<template>
  <div class="hello">
    <body>
      <section>
        <button id="login">ログイン</button>
        <button id="logout">ログアウト</button>
      </section>

      <button @click="api()"></button>

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
    data: () => ({
      responsedata: '',
      Picasa: null,
      picasa: null,
      accessToken: null,
      config: {
        clientId: '1030214100740-ji93rpfe48k1rnsufifi9q4b5vh7urc8.apps.googleusercontent.com',
        redirectURI: 'https://daiki-photo.web.app/',
        clientSecret: 'IJdpDEl7kJY_6hutq1C5lGZK'
      },
      refreshToken: 'yourRefreshToken',
      apiKey: 'AIzaSyAt4zFCrqpQzUt_psIRRqv2l7tBUEVoywc',
      clientId: '1030214100740-ji93rpfe48k1rnsufifi9q4b5vh7urc8.apps.googleusercontent.com',
      discoveryDocs: [],
      scopes: 'https://www.googleapis.com/auth/photoslibrary',
      authorizeButton: null,
      signoutButton: null,
      gapi: null,
      files: [],
    }),
    created() {
      this.Picasa = require('picasa');
      this.picasa = new this.Picasa();
    },
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
    methods: {
      pullPhotoUrl () {
      // アクセストークンを取得します。
      this.picasa.renewAccessToken(this.config, this.refreshToken)
      .then(token => {
        this.accessToken = token;
        // アルバム一覧を取得します。
        return this.picasa.getAlbums(this.accessToken, null);
      })
      .then(albums => {
        // アルバム内の写真一覧を取得します。
        return Promise.all(albums.map(album => {
          return this.picasa.getPhotos(this.accessToken, {albumId: album.id});
        }));
      })
      .then(albumResults => {
        for (const photos of albumResults) {
          for (const photo of photos) {
            console.log(`Content-Type: ${photo.content.type}, URL: ${photo.content.src}`)
          }
        }
      })
      .catch(error => {
        console.log(error);
      });
    },
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
      })
      .then(() => {
        this.pullPhotoUrl()
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
      api(){
        var axios = require('axios');
        var data = 'grant_type=password&username=qhp4502q5er245@print.epsonconnect.com&password=';
        var config = {
          method: 'post',
          url: 'https://api.epsonconnect.com/api/1/printing/oauth2/auth/token?subject=printer',
          headers: { 
            'Content-Type': 'application/x-www-form-urlencoded; charset=utf-8', 
            'Authorization': 'Basic ZmZiMWYwMjhmZDQzNGZhYmFhNWMyYmNjN2VhOWY1NzI6YkN1YTllN1pESlRYcnAwZ3hDYzF3SVpLZTN2NUNnbDZobXBaYW1pdDVPSGVtUUFWTVJRMzRVUGc5aHNPZU04aw=='
          },
          data : data
        };

        axios(config)
        .then(response => {
          const obj = response.data;
          this.responsedata = obj;
          console.log(this.responsedata.access_token)
        })
        .catch( error => {
        console.log(error);
        });
        this.api2();
      },
      api2(){
          var axios = require('axios');
          var data = '{\r\n    "job_name" : "Job Name",\r\n    "print_mode" : "document",\r\n    "print_setting" : {\r\n        "media_size" : "ms_a4",\r\n        "media_type" : "mt_plainpaper",\r\n        "borderless" : false,\r\n        "print_quality" : "normal",\r\n        "source" : "auto",\r\n        "color_mode" : "color",\r\n        "reverse_order" : false,\r\n        "copies" : 1,\r\n        "collate" : false\r\n    }\r\n}';
          var config = {
            method: 'post',
            url: 'https://api.epsonconnect.com/api/1/printing/printers/8f124241fc7046cebbd0abd4545c7672/jobs',
            headers: { 
              'Content-Type': 'application/json; charset=utf-8', 
              'Authorization': this.responsedata.token_type+" "+this.responsedata.access_token
            },
            data : data
          };
          axios(config)
          .then(function (response) {
            console.log(JSON.stringify(response.data));
          })
          .catch(function (error) {
            console.log(error);
          });
        this.api3();
      },
      api3(){
        var axios = require('axios');
        var data = '1.png';

        var config = {
          method: 'post',
          url: 'https://www.epsonconnect.com/c33fe124ef80c3b13670be27a6b0bcd7/v1/storage/PostData?Key=ceee70d8d80d64ecef98b9782751b6c6a2fac953783030627f53694863b658ec79f8b2ba024054fd&File=1.pdf',
          headers: { 
            'Content-Length': '50589', 
            'Content-Type': 'application/octet-stream', 
            'Authorization': this.responsedata.token_type+" "+this.responsedata.access_token
          },
          data : data
        };

        axios(config)
        .then(function (response) {
          console.log(JSON.stringify(response.data));
        })
        .catch(function (error) {
          console.log(error);
        });
        this.api4();
      },
      api4(){
        var axios = require('axios');
        var data = '1.png';

        var config = {
          method: 'post',
          url: 'https://api.epsonconnect.com/api/1/printing/printers/8f124241fc7046cebbd0abd4545c7672/jobs/89d5f64845e94387907bc9ba22a84ad4/print',
          headers: { 
            'Content-Type': '0', 
            'Authorization': this.responsedata.token_type+" "+this.responsedata.access_token
          },
          data : data
        };

        axios(config)
        .then(function (response) {
          console.log(JSON.stringify(response.data));
        })
        .catch(function (error) {
          console.log(error.response);
        });
    }
  }
}
</script>
