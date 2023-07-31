<script setup>
import { io } from 'socket.io-client';
import { onBeforeMount, ref } from 'vue';

const socket = io('http://localhost:3000');

const messages = ref([]);
const messageText = ref('');
const joined = ref(false);
const name = ref('');

onBeforeMount(() => {
    socket.emit('findAllMessages', {}, (response) => {
        messages.value = response;
    });

    socket.on('message', (message) => {
        messages.value.push(message);
    })
})

const sendMessage = () => {
    socket.emit('createMessage', { text: messageText.value }, () => {
        messageText.value = '';
    })
}

const join = () => {
    socket.emit('join', { name: name.value }, () => {
        joined.value = true;
    })
}



</script>

<template>
    <div class="chat">
        <div class="chat-container">
            <div class="messages-container">
                <div v-for="message in messages">
                    [{{ message.name }}]: {{ message.text }}
                </div>
            </div>
        </div>
    </div>
  
</template>

<style>
@import './assets/base.css';

</style>
