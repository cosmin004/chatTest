<template>
  <div id="app">
    <div v-if="userSelected && channelSelected" id="chat">
      <Messages :user="userId" :userName="userName" :channelName="channelName" :channelId="channelId"/>
      <chatInput :user="userId" :userName="userName" :channelName="channelName" :channelId="channelId"/>
    </div>
    <div id="channels" v-else-if="userSelected">
      <channelSelection :userId="userId"/>
    </div>
    <div v-else>
      <userSelection/>
    </div>
  </div>
</template>

<script>
import chatInput from './components/chatInput.vue'
import Messages from './components/ReceivedMessages.vue'
import userSelection from './components/userSelection.vue'
import channelSelection from './components/channels.vue'

export default {
  name: 'app',
  components: {
    Messages,
    chatInput,
    userSelection,
    channelSelection
  },
  data: function() {
    return {
      userId : '',
      userName: '',
      channelId: '',
      channelName: '',
      usersInChannel: '',
      userSelected: false,
      channelSelected: false,
    }
  },
  methods:{
    setUser: function(user){
      this.userId = user.id.S;
      this.userName = user.name.S;
      this.userSelected = true;
    },
    setChannel: function(channel){
      this.channelId = channel.id.S;
      this.channelName = channel.name.S;
      this.usersInChannel = channel.usersInChannel.SS;
      this.channelSelected = true;
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#chat {
  height:90vh; 
}
</style>
