<template>
  <v-app>
    <div v-if="loggedIn">
      <Main_form :userName="userName"/>
    </div>
    <div v-else>
      <Login_form v-on:logStsChange="logStsChange"/>
    </div>
    

  </v-app>
    
</template>

<script>
import Login_form from './components/Login';
import Main_form from './components/Main';


export default {
  name: 'App',

  components: {
    Login_form,
    Main_form
  },

  data: () => ({
    loggedIn: false,
    userName: ''
  }),
  methods:{
    logStsChange(name){
      this.loggedIn = !this.loggedIn
      this.userName = name
    }
  }
};
</script>

<style lang="scss">
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}
::-webkit-scrollbar-track {
  background: transparent
}
::-webkit-scrollbar-thumb {
  background: #727780;
  border-radius: 3px;
  margin-top: 25px;
  margin-bottom: 25px;
}
::-webkit-scrollbar-thumb:active {
  background: #666b74;
  border-radius: 10px;
}
</style>