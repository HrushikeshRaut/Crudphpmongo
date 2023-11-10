<template>
  <div id="app">
    <AdmiForm />
    
    <UserData  />
  </div>
</template>

<script>
import AdmiForm from './components/AdmiForm.vue';
import UserData from './components/UserData.vue';

export default {
  name: 'App',
  components: {
    AdmiForm,
    UserData,
  },

  // data() {
  //   return {
  //   userData: [],
  //   };
  // },

  // methods:{
  //   updateUserData(data) {
  //     this.userData = data ;
  //   }
  // }
}
</script>

<style>

body {
  background-color: #EEEEEE;
  font-family: 'Montserrat', sans-serif;
  display: grid;
  grid-template-rows: auto;
  justify-items: center;
  padding-top: 30px;
}

body, html {
  margin: 0;
  height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
</style>
