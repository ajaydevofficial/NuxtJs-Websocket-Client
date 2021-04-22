<template>
    <div class="container mt-4">
        <h3>Chat Room Provider</h3>
        <div class="chat-room-selector">
            <div class="alert alert-secondary">
                <p class="m-0">Provide existing chat room name to join or new room name to create</p>
            </div>
            <input v-on:keyup.enter="joinChat()" class="form-control" type="text" placeholder="Enter chat room" v-model="chatRoomName">
            <p class="text-danger mt-3 mb-0" v-if="error">{{error}}</p>
            <button class="mt-3 btn btn-primary" @click="joinChat()">Join/Create Room</button>
        </div>
    </div>
</template>

<script>
export default {
    mounted(){

    },
    methods:{
        joinChat(){
            if(this.chatRoomName){
                this.error = null;
                this.$router.push(`/chat?room=${this.chatRoomName}`);
            }
            else{
                this.error = "Chat room name cannot be empty";
            }
        }
    },
    data(){
        return {
            chatRoomName:'',
            error: null,
        }
    }
}
</script>

<style>

</style>