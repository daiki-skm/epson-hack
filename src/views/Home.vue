<template>
  <div class="home">
    <main>
      <button @click="api()">First phase</button>
      <button @click="api2()">Second phase</button>
      <button @click="api3()">Third phase</button>
      <button @click="api4()">Final phase</button>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',

  components: {
  },

  data: () => ({
    responsedata:'',
    actoken:''
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
        console.log(error);
        });

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