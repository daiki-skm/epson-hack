<template>
  <!-- <div class="hello">
    <body>
      <h1>Upload Sample</h1>
      <section>
        <button id="login" onclick="login()">ログイン</button>
        <button id="logout" onclick="logout()">ログアウト</button>
      </section>

      <section>
        <div id="file" style="margin-top: 20px;">
          <input id="file-input" type="file" onchange="upload(this.files[0])" />
        </div>
      </section>
      <div id="daiki" />
    </body>

    <h1>だいき</h1>
    <h1>daiki-checkout</h1>
  </div> -->
  <div class="container">
    <!--Add buttons to initiate auth sequence and sign out-->
    <button id="authorize_button">Authorize</button>
    <button id="signout_button">Sign Out</button>
    <pre id="content" style="white-space: pre-wrap;"></pre>
  </div>
</template>

<script>
// import API from "./api"

export default {
  name: 'HelloWorld',
  components:{
    // API
  },
  props: {
    msg: String
  },
  data: () => ({
    apiKey: 'AIzaSyDtmgKef90c4WVsw5KzQ9pR_kRdz8oACM0',
    clientId: '1030214100740-hj8tj2jfiurrefrpogkcojrl5qr5bg9r.apps.googleusercontent.com',
    discoveryDocs: [],
    scopes: 'https://www.googleapis.com/auth/photoslibrary',
    authorizeButton: null,
    signoutButton: null,
    gapi: null,
  }),
  mounted() {
    this.authorizeButton = document.getElementById('authorize_button');
    this.signoutButton = document.getElementById('signout_button');
    // app.blame.php で api.js を取得する前に mounted が実行される可能性があるため 
    // setInterval と if(window.gapi) で確認する
    const id = setInterval(() => {
      if(window.gapi){
        this.gapi = window.gapi;
        this.handleClientLoad();
        clearInterval(id);
      }
    }, 1000);
    /* const sub = document.getElementById("daiki")
    let recaptchaScript = document.createElement('script')
    recaptchaScript.setAttribute('src', 'https://www.google.com/recaptcha/api.js')
    sub.appendChild(recaptchaScript)
    console.log(recaptchaScript.setAttribute('src', 'https://www.google.com/recaptcha/api.js')) */
    // onload="this.onload=function(){};handleClientLoad()"
    // onreadystatechange="if (this.readyState === 'complete') this.onload()"
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
          // Listen for sign-in state changes.
          this.gapi.auth2.getAuthInstance().isSignedIn.listen(this.updateSigninStatus);
          // // Handle the initial sign-in state.
          this.updateSigninStatus(this.gapi.auth2.getAuthInstance().isSignedIn.get());
          this.authorizeButton.onclick = this.handleAuthClick;
          this.signoutButton.onclick = this.handleSignoutClick;
        }, (error) => {
          this.appendPre(JSON.stringify(error, null, 2));
        });
      },
      updateSigninStatus(isSignedIn) {
        if (isSignedIn) {
          this.authorizeButton.style.display = 'none';
          this.signoutButton.style.display = 'block';
          this.listFiles();
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
      },
      appendPre(message) {
        const pre = document.getElementById('content');
        const textContent = document.createTextNode(message + '\n');
        pre.appendChild(textContent);
      },
      listFiles() {
        this.gapi.client.drive.files.list({
          'pageSize': 10,
          'fields': "nextPageToken, files(id, name)"
        }).then((response) => {
            this.appendPre('Files:');
            const files = response.result.files;
            if (files && files.length > 0) {
              for (let i = 0; i < files.length; i++) {
                let file = files[i];
                this.appendPre(file.name + ' (' + file.id + ')');
              }
            } else {
              this.appendPre('No files found.');
            }
        });
      },
    // Google API Client Library for JavaScript ロード時のイベント
    /* handleClientLoad() {
      gapi.load('client:auth2', () => {
        gapi.client.init({
          apiKey: this.apiKey,
          discoveryDocs: this.discoveryDocs,
          clientId: this.clientId,
          scope: this.scopes
        }).then(function () {
          // サインイン状態を監視し、状態に変化があったときに「updateSigninStatus」を呼ぶ
          gapi.auth2.getAuthInstance().isSignedIn.listen(updateSigninStatus);
          // 初期起動時のサインイン状態で画面制御
          updateSigninStatus(gapi.auth2.getAuthInstance().isSignedIn.get());
        });
      });
    },
    // ログイン
    login() {
      gapi.auth2.getAuthInstance().signIn();
    },
    // ログアウト
    logout() {
      gapi.auth2.getAuthInstance().signOut();
      gapi.auth2.getAuthInstance().disconnect();
    },
    // ログイン・ログアウト状態に変更が発生した時に呼ばれる関数
    updateSigninStatus(isSignedIn) {
      if (isSignedIn) {
        //ログイン状態
        document.getElementById("login").style.display = 'none';
        document.getElementById("logout").style.display = 'block';
        document.getElementById("file").style.display = 'block';
      } else {
        //ログアウト
        document.getElementById("login").style.display = 'block';
        document.getElementById("logout").style.display = 'none';
        document.getElementById("file").style.display = 'none';
      }
    },
    // 選択したファイルをGoogleフォトへアップルードする
    upload(file) {
      var accessToken = gapi.auth.getToken().access_token; // OAuthアクセスキーを取得

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
        return batchCreate(token);
      });
    },
    // Media Batch Create APIを呼び出し、Googleフォトへアップロードしたファイルを登録
    batchCreate(uploadToken) {
      // gapiで、Media Batch Create API呼び出しリクエストの作成
      var restRequest = gapi.client.request({
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
    }, */
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
