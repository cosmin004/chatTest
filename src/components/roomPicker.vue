<template>
    <div id="app">
        <h2>Select a Channel to begin with</h2>
        <div v-for="(channel, index) in existingChannels" :key="channel.id.S">
            <div v-if="channel.name" class="listItem">
                <input type="radio" name="channel" :value="index" v-model="selectedIndex">{{channel.name.S}}
            </div>
        </div>
        <button v-on:click="callSetChannel(existingChannels[selectedIndex])">Select</button>
        <button v-on:click="$parent.goToCreate()">Create</button>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'roomPick',
    beforeMount: function(){
        var obj = this;
        axios.get(
            'http://app-prod.b4cjkb3nwe.eu-west-1.elasticbeanstalk.com/chat/getChannels/'
        )
        .then(function(response){
            obj.existingChannels = response.data;
        })
        .catch(function(response){
            console.log(response.error);
        });
    },
    data: function(){
        return {
            existingChannels:[

            ],
            selectedIndex: ''
        }
    },
    methods: {
        callSetChannel: function(channel){
            this.$parent.$parent.setChannel(channel);
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
        margin-left: 5px;
    }
    .listItem{
        padding: 5px;
        display: grid;
        border-bottom: 1px solid rgba(0, 0, 0, 0.1);
        grid-template-columns: 1fr 9fr;
        text-align: left
    }
</style>