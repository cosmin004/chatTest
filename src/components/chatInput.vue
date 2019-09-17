<template>
  <div id="app">
      <input type="text" name="message" id="message" v-model="message" placeholder="Type message here..." v-on:keyup.enter="sendMessage" autocomplete="off"/>
      <button v-on:click="sendMessage"><i class="arrow right"></i></button>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'chatInput',
    props:[
        'user',
        'userName',
        'channelName',
        'channelId'
    ],
    data: function() {
        return{
            message: '',
            // channelId: '8dcc6fc3-f22a-48fc-8f99-0c7fee3cbf3c',
            // channelName: 'test-channel',
        }
    },
    methods:{
        sendMessage:function(){
            if(this.message != '') {
                axios.post('http://app-prod.b4cjkb3nwe.eu-west-1.elasticbeanstalk.com/chat/', {
                    message: this.message,
                    userId: this.user,
                    userName: this.userName,
                    channelId: this.channelId,
                    channelName: this.channelName
                })
                .then(function (response) {
                    console.log(response.data)
                })
                .catch(function (error) {
                    console.log(error);
                });
            }
            this.message = '';
        }
    }
}
</script>

<style scoped>
    #app {
        margin-top: 0px;
        background-color: rgba(0, 0, 0, 0.1);
        font-size: 32px;
        padding: 10px;
    }
    #app > input {
        width: 80%;
        font-size: 12px;
        border-radius: 25px;
        border: 0px;
        padding: 15px; 
    }
    #app > input:focus {
        outline: none;
    }
    #app > button {
        margin-left: 10px;
        background-color: rgba(0, 0, 0, 0.0);
        border: none;
        padding: 15px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
        cursor: pointer;
    }
    i {
    border: solid rgba(0, 0, 0, 0.4);
    border-width: 0 3px 3px 0;
    display: inline-block;
    padding: 5px;
    }

    .right {
    transform: rotate(-45deg);
    -webkit-transform: rotate(-45deg);
    }

</style>