<template>
  <div class="container mt-4">
    <h3>Chat Room : {{this.$route.query.room}}</h3>
    <p v-if="error" class="small text-danger">{{error}}</p>
    <div class="chat-room-message-input">
        <input v-on:keyup.enter="sendMessage()" class="p-2 w-100" type="text" placeholder="Send message" v-model="message">
    </div>
    <hr>
    <h5 class="mt-4">Chat Logs</h5>
    <div class="chat-logs mt-3">
      <div v-if="chatLogs.length>0">
        <p v-bind:key="index" v-for="(log,index) in chatLogs" class="small">{{log}}</p>
      </div>
      <div v-else>
        <p class="small">No messages yet</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted(){
    this.chatSocket = new WebSocket(`ws://${window.location.host}/ws/chat/${this.$route.query.room}/`);
    const self = this;
    this.chatSocket.onmessage = function(e) {
        const data = JSON.parse(e.data);
        self.chatLogs.push(data.message);
    };

    this.chatSocket.onclose = function(e) {
        console.error('Chat socket closed unexpectedly');
    };
  },
  methods: {
    sendMessage(){
      if(this.message){
        this.error = null;
        this.chatSocket.send(JSON.stringify(
          {
            message: this.message
          }
        ))
        this.message = null;
      }
      else{
        this.error = "Message cannot be empty";
      }
    }
  },
  data(){
    return {
      chatLogs: [],
      chatSocket: null,
      message: null,
      error: null
    }
  }
}

</script>

<style>

</style>
