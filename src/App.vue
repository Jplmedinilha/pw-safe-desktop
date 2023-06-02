<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
          @click="teste"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        @click="teste"
      >
        oi
      </v-btn>
    </v-app-bar>

    <v-main>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';
// import authHeader from "../axios/auth-header";
import authHeader from "./axios/auth-header";


export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    teste_5: []
  }),
  methods:{
    async teste(){

      await this.axios.get(
        `http://localhost:3000/api/login?username=JMEDINILHA&pw=jplm12345`, { headers: authHeader(true) }
      )
      .then((res) =>{
        console.log(res)
      })
      .catch((err)=>{
        console.log(err)
      })

      console.log('oi');
    }
  }
};
</script>
