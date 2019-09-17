<template>
  <div id="app">
        <h2>Create new channel</h2>
        <label for="channelName">Name</label>
        <input type="text" name="channelName" id="channelName" v-model="channelName">
        <br>
        <h3>Select users for this channel</h3>
        <div v-for="user in users" :key="user.id.S" class="usersSelection">
            <div>
                <input type="checkbox" name="" id="" :value="user.id.S" v-model="selectedUsers"/>
            </div>
            <div>
                {{user.name.S}}
            </div>
        </div>
        <button v-on:click="registerChannel()">Create</button>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'createNewChannel',
    props:[
        'userId'
    ],
    beforeMount: function(){
        var obj = this;

        axios.get(
            'http://app-prod.b4cjkb3nwe.eu-west-1.elasticbeanstalk.com/chat/getUsers/'
        )
        .then(function(response){
            obj.users = response.data.filter(user => user.name && user.id.S != obj.userId);
        })
        .catch(function(response){
            console.log(response.error);
        });
    },
    data: function(){
        return {
            users:[
            ],
            channelName: '',
            selectedUsers:[
            ]
        }
    },
    methods:{
        registerChannel:function(){
            if(this.selectedUsers.length && this.channelName != ''){
                this.selectedUsers.push(this.userId);
                var obj = this;
                axios.post(
                    'http://app-prod.b4cjkb3nwe.eu-west-1.elasticbeanstalk.com/chat/createChannel/',
                    {
                        channelName : this.channelName,
                        usersInChannel: this.selectedUsers
                    }
                )
                .then(function(response){
                    obj.$parent.$parent.setChannel({
                        'id': {'S':response.data.channelId},
                        'name': {'S':obj.channelName},
                        'usersInChannel':{'SS': obj.selectedUsers}
                    });
                })
                .catch(function(response){
                    console.log(response.error);
                });
            }
        }
    }
}
</script>

<style scoped>
    .usersSelection{
        display: grid;
        grid-template-columns: 1fr 9fr;
        text-align: left;
    }
    #app{
        background-color: rgba(223, 222, 221, 0.308);
        width: 30%;
        margin-left: 35%;
        padding: 10px;
    }
    button{
        margin-top: 10px;
        margin-bottom: 5px;
        margin-left: 5px;
    }
</style>