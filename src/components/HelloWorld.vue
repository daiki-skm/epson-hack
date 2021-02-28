<template>
<<<<<<< HEAD
  <div class="hello">
    <body>
      <img v-if="show" :src="textareavalue"> 
      <section>
        <v-btn @click="test()">💩</v-btn>
        <v-btn id="login">ログイン</v-btn>
        <v-btn id="logout">ログアウト</v-btn>
      </section>

      <section> 
        <div id="file" style="margin-top: 20px;">
          <input type="file" id="file-input" ref="myFiles" @change="previewFiles" multiple>
        </div>
      </section>
    </body>
  </div>
=======
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img
          :src="require('../assets/logo.svg')"
          class="my-3"
          contain
          height="200"
        />
      </v-col>

      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to Vuetify
        </h1>

        <p class="subheading font-weight-regular">
          For help and collaboration with other Vuetify developers,
          <br>please join our online
          <a
            href="https://community.vuetifyjs.com"
            target="_blank"
          >Discord Community</a>
        </p>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          What's next?
        </h2>

        <v-row justify="center">
          <a
            v-for="(next, i) in whatsNext"
            :key="i"
            :href="next.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ next.text }}
          </a>
        </v-row>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          Important Links
        </h2>

        <v-row justify="center">
          <a
            v-for="(link, i) in importantLinks"
            :key="i"
            :href="link.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ link.text }}
          </a>
        </v-row>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          Ecosystem
        </h2>

        <v-row justify="center">
          <a
            v-for="(eco, i) in ecosystem"
            :key="i"
            :href="eco.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ eco.text }}
          </a>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
>>>>>>> 624a02ba1b130a9cd06fe25443fea14a6d8ef498
</template>

<script>
  export default {
    name: 'HelloWorld',

<<<<<<< HEAD
export default {
  name: 'googlePhoto',
  components:{
  },
  props: {
    msg: String
  },
  data: () => ({
    getUrl: 'https://photos.app.goo.gl/6nNMr6gmhpHZnT8H8',
    textareavalue: "",
    show: true,
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
  watch: {
  },
  methods: {
    test(){
      this.textareavalue = "https://chart.googleapis.com/chart?cht=qr&chs=200x200&chl=" + this.getUrl + "&choe=UTF-8QRCode"
      this.show = !this.show
    },
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
=======
    data: () => ({
      responsedata:'',
      ecosystem: [
        {
          text: 'vuetify-loader',
          href: 'https://github.com/vuetifyjs/vuetify-loader',
        },
        {
          text: 'github',
          href: 'https://github.com/vuetifyjs/vuetify',
        },
        {
          text: 'awesome-vuetify',
          href: 'https://github.com/vuetifyjs/awesome-vuetify',
        },
      ],
      importantLinks: [
        {
          text: 'Documentation',
          href: 'https://vuetifyjs.com',
        },
        {
          text: 'Chat',
          href: 'https://community.vuetifyjs.com',
        },
        {
          text: 'Made with Vuetify',
          href: 'https://madewithvuejs.com/vuetify',
        },
        {
          text: 'Twitter',
          href: 'https://twitter.com/vuetifyjs',
        },
        {
          text: 'Articles',
          href: 'https://medium.com/vuetify',
        },
      ],
      whatsNext: [
        {
          text: 'Explore components',
          href: 'https://vuetifyjs.com/components/api-explorer',
        },
        {
          text: 'Select a layout',
          href: 'https://vuetifyjs.com/getting-started/pre-made-layouts',
        },
        {
          text: 'Frequently Asked Questions',
          href: 'https://vuetifyjs.com/getting-started/frequently-asked-questions',
        },
      ],
    }),
     methods: {
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
>>>>>>> 624a02ba1b130a9cd06fe25443fea14a6d8ef498
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
