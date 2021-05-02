<template>
    <div class="mb-3">
        <b-navbar class="d-flex align-items-center justify-content-end p-0">
            <span class="text-dark">Notifications <b-badge variant="danger">{{notifications.length}}</b-badge></span>
        </b-navbar>
    </div>
</template>

<script>
export default {
    mounted(){
        this.chatSocket = new WebSocket(`ws://${window.location.host}/ws/notifications`);
        const self = this;
        this.chatSocket.onmessage = function(e) {
            const data = JSON.parse(e.data);
            console.log(data)
            self.notifications.push(data)
        };

        this.chatSocket.onclose = function(e) {
            console.error('Chat socket closed unexpectedly');
        };
    },
    data(){
        return{
            chatSocket: null,
            notifications: []
        }
    }
}
</script>

<style>

</style>