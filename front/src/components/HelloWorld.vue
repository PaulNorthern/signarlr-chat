<template>
  <div>
    <div v-for="message in messages" :key="message.id">
      <strong>{{ message.user }}:</strong> {{ message.text }}
    </div>
    <div v-on:keyup.enter="send">
      <input v-model="user" placeholder="Username">
      <input v-model="text" placeholder="Message">
      <button @click="send">Send</button>
    </div>
  </div>
</template>

<script>
import * as signalR from '@aspnet/signalr';

export default {
  data() {
    return {
      user: '',
      text: '',
      messages: []
    }
  },
  mounted() {
    const connection = new signalR.HubConnectionBuilder()
      .withUrl("http://localhost:5152/chatHub")
      .build();

    connection.on("ReceiveMessage", (user, message) => {
      this.messages.push({ user: user, text: message });
    });

    connection.start()
      .catch(err => console.error(err.toString()));
  },
  methods: {
    send() {
      const connection = new signalR.HubConnectionBuilder()
        .withUrl("http://localhost:5152/chatHub")
        .build();

      connection.start()
        .then(() => {
          connection.invoke("SendMessage", this.user, this.text);
          this.text = '';
        })
        .catch(err => console.error(err.toString()));
    }
  }
}
</script>
