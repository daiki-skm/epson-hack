<template>
  <div class="hello">
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
      <!-- <script async defer src="https://apis.google.com/js/api.js" onload="this.onload=function(){};handleClientLoad()" onreadystatechange="if (this.readyState === 'complete') this.onload()">
      </script> -->
    </body>

    <h1>だいき</h1>
    <h1>daiki-checkout</h1>
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-router" target="_blank" rel="noopener">router</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-vuex" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
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
    judge: false,
  }),
  mounted() {
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
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
