<template>
  <div class="hello">
    <body>
      <section>
        <button id="login">ログイン</button>
        <button id="logout">ログアウト</button>
      </section>

      <section>
        <div id="file" style="margin-top: 20px;">
          <input type="file" id="file-input" ref="myFiles" @change="previewFiles" multiple>
        </div>
      </section>
    </body>
  </div>
</template>

<script>

export default {
  name: 'HelloWorld',
  components:{
  },
  props: {
    msg: String
  },
  data: () => ({
    apiKey: 'AIzaSyAt4zFCrqpQzUt_psIRRqv2l7tBUEVoywc',
    clientId: '1030214100740-ji93rpfe48k1rnsufifi9q4b5vh7urc8.apps.googleusercontent.com',
    discoveryDocs: [],
    scopes: 'https://www.googleapis.com/auth/photoslibrary',
    authorizeButton: null,
    signoutButton: null,
    gapi: null,
    files: [],
  }),
  mounted() {
    this.authorizeButton = document.getElementById('login');
    this.signoutButton = document.getElementById('logout');
    // setInterval と if(window.gapi) で確認する
    const id = setInterval(() => {
      if(window.gapi){
        this.gapi = window.gapi;
        this.handleClientLoad();
        clearInterval(id);
      }
    }, 1000);
  },
  watch: {
  },
  methods: {
    handleClientLoad () {
      this.gapi.load('client:auth2', this.initClient);
    },
    initClient() {
      this.gapi.client.init({
        apiKey: this.apiKey,
        clientId: this.clientId,
        discoveryDocs: this.discoveryDocs,
        scope: this.scopes,
      }).then(() => {
        // サインイン状態を監視し、状態に変化があったときに「updateSigninStatus」を呼ぶ
        this.gapi.auth2.getAuthInstance().isSignedIn.listen(this.updateSigninStatus);
        // 初期起動時のサインイン状態で画面制御
        this.updateSigninStatus(this.gapi.auth2.getAuthInstance().isSignedIn.get());
        this.authorizeButton.onclick = this.handleAuthClick;
        this.signoutButton.onclick = this.handleSignoutClick;
      });
    },
    updateSigninStatus(isSignedIn) {
      if (isSignedIn) {
        this.authorizeButton.style.display = 'none';
        this.signoutButton.style.display = 'block';
      } else {
        this.authorizeButton.style.display = 'block';
        this.signoutButton.style.display = 'none';
      }
    },
    handleAuthClick() {
      this.gapi.auth2.getAuthInstance().signIn();
    },
    handleSignoutClick() {
      this.gapi.auth2.getAuthInstance().signOut();
      this.gapi.auth2.getAuthInstance().disconnect();
    },
    previewFiles() {
      this.files = this.$refs.myFiles.files
      console.log(this.files[0])
      this.upload(this.files[0])
    },
    // 選択したファイルをGoogleフォトへアップルードする
    upload(file) {
      var accessToken = this.gapi.auth.getToken().access_token; // OAuthアクセスキーを取得

      // Media Upload APIでファイルをアップロード
      fetch('https://photoslibrary.googleapis.com/v1/uploads', {
        method: 'POST',
        headers: new Headers({ 
          'Authorization': 'Bearer ' + accessToken,
          "Content-type": "application/octet-stream",
          "X-Goog-Upload-Content-Type": file.type,
          "X-Goog-Upload-Protocol": "raw"
        }),
        body: file,
      })
      .then(res => res.text())
      .then(token => {
        // レスポンスのUploadTokenをパラメータにbatchCreateを呼ぶ
        return this.batchCreate(token);
      });
    },
    // Media Batch Create APIを呼び出し、Googleフォトへアップロードしたファイルを登録
    batchCreate(uploadToken) {
      // gapiで、Media Batch Create API呼び出しリクエストの作成
      var restRequest = this.gapi.client.request({
        'path': 'https://photoslibrary.googleapis.com/v1/mediaItems:batchCreate',
        'method': 'POST',
        'body': {
          'newMediaItems' : [
            {
              'description': "test batch create", // ファイルの概要
              'simpleMediaItem': {
                'uploadToken': uploadToken,  // Media Upload APIのレスポンスで取得したUploadTokenをセット
                'fileName': 'test file name' // ファイル名
              }
            }
          ]
        }
      });
      // リクエストの実行
      restRequest.execute((resp) => {
        console.log(resp);
      });
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
