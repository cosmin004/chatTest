<template>
    <div id="app">
        <h2>{{channelName}}</h2>
        <div class="messages">
            <div v-for="message in messages" :key="message.content.S">
                <div class="name ntr" v-if="message.userId.S==user">
                    You
                </div>
                <div class="name ntl" v-else>
                    {{message.userName.S}}
                </div>
                <div class="box sb1" v-if="message.userId.S==user">{{message.content.S}}</div>
                <div class="box sb2" v-else>{{message.content.S}}</div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import Pusher from 'pusher-js'

export default {
    name: 'messages',
    props:[
        'user',
        'userName',
        'channelName',
        'channelId'
    ],
    beforeMount: function(){
        var pusher = new Pusher('e46488b1793af8778b05', {
            cluster: 'eu',
            forceTLS: true
        });
        var channel = pusher.subscribe(this.channelName);
        var obj = this;
        
        axios.get(
            'http://app-prod.b4cjkb3nwe.eu-west-1.elasticbeanstalk.com/chat/getMessages/?channelId='+this.channelId
        )
        .then(function (response) {
            obj.messages = response.data;
        })
        .catch(function (error) {
            console.log(error);
        })

        channel.bind('my-event', function(data) {
            obj.addMessage(data);
        });
    },
    data: function() {
        return{
            messages : [
                
            ],
        }
    },
    methods:{
        addMessage : function(data){
            this.messages.push({
                'content':{'S':data.message},
                'userName':{'S':data.userName},
                'userId':{'S':data.userId}
            });
        }
    }
}
</script>
<style scoped>
    .messages {
        max-height: 96.4%;
        overflow:auto;
    }
    h2 {
        margin: 0;
        border-bottom: 1px solid rgba(0, 0, 0, 0.1);
        margin-bottom: 5px;
    }
    #app {
        border: 1px solid rgba(0, 0, 0, 0.1);
        padding: 10px; 
        height: 90%;
    }
    .name {
        font-size: 12px;
        font-weight: bold;
    }

    .ntl {
        text-align: left;
    }

    .ntr {
        text-align: right;
    }

    .box {
    width: 300px;
    margin: 50px auto;
    margin-top: 5px;
    background: rgba(215, 247, 197);
    padding: 20px;
    text-align: left;
    font-weight: 900;
    color: rgb(0, 0, 0);
    font-family: arial;
    position:relative;
    margin-bottom: 0px;
    }

    .box:last-of-type {
        margin-bottom: 20px;
    }

    .sb1:before {
    content: "";
    width: 0px;
    height: 0px;
    position: absolute;
    border-left: 10px solid rgba(215, 247, 197);
    border-right: 10px solid transparent;
    border-top: 10px solid rgba(215, 247, 197);
    border-bottom: 10px solid transparent;
    right: -19px;
    top: 6px;
    }

    .sb1 {
        margin-right: 30px;
    }

    .sb2:before {
    content: "";
    width: 0px;
    height: 0px;
    position: absolute;
    border-left: 10px solid transparent;
    border-right: 10px solid rgba(215, 247, 197);
    border-top: 10px solid rgba(215, 247, 197);
    border-bottom: 10px solid transparent;
    left: -19px;
    top: 6px;
    }
    .sb2 {
        margin-left: 30px;
    }
</style>