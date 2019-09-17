<template>
  <div id="app">
    <createChannel v-if="createNew" :userId="userId"/>
    <roomPicker v-else/>
  </div>
</template>

<script>
import roomPicker from './roomPicker.vue'
import createChannel from './createChannel.vue'

export default {
  name: 'channels',
  components: {
    roomPicker,
    createChannel
  },
  props: [
    'userId'
  ],
  data: function(){
    return {
      createNew: false
    }
  },
  methods: {
    goToCreate: function(){
      this.createNew = true
    }
  }
}
</script>

<style>

</style>