<template>
  <div id="app">
      <h2>Select a User to begin with</h2>
      <div v-for="(user, index) in users" :key="user.id.S">
          <div v-if="user.name" class="listItem">
              <input type="radio" name="user" :value="index" v-model="selectedIndex">{{user.name.S}}
          </div>
      </div>
      <button v-on:click="callSetUser(users[selectedIndex])">Select</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'userSelection',
    beforeMount: function(){
        var obj = this;
        axios.get(
            'http://app-prod.b4cjkb3nwe.eu-west-1.elasticbeanstalk.com/chat/getUsers/'
        )
        .then(function (response) {
            obj.users = response.data;
        })
        .catch(function (error) {
            console.log(error);
        })
        
    },
    data: function() {
        return{
            users:[
            ],
            selectedIndex: ''
        }
    },
    methods: {
        callSetUser: function(user){
            this.$parent.setUser(user);
        }
    }
}
</script>

<style scoped>
    #app{
        background-color: rgba(223, 222, 221, 0.308);
        width: 30%;
        margin-left: 35%;
        padding: 10px;
    }
    button{
        margin-top: 10px;
        margin-bottom: 5px;
    }
    .listItem{
        padding: 5px;
        display: grid;
        border-bottom: 1px solid rgba(0, 0, 0, 0.1);
        grid-template-columns: 1fr 9fr;
        text-align: left
    }
</style>