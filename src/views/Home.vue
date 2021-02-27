<template>
  <div class="home">
    <main>
      <v-btn @click="api()">api</v-btn>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',

  components: {
  },

  data: () => ({
    //
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
        .then(function (response) {
          console.log(JSON.stringify(response.data));
        })
        .catch(function (error) {
          console.log(error);
        });

    }
  }
}
</script>