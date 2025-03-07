<template>
  <h2 class="title">
    Chat <img src="../assets/msg.png" alt="icon message" />
  </h2>

  <div class="chat">
    <div class="messages" ref="messageContainer">
      <div
        v-for="(message, id) in messages"
        :key="id"
        :class="['message', message.user === username ? 'my-message' : 'other-message']"
      >
        <div class="user-name">{{ message.user }}</div>
        <div class="user-text">{{ message.text }}</div>
        <span class="time">{{ message.time }}</span>
      </div>
    </div>

    <div class="input-container">
      <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="your message..." />
      <button @click="sendMessage"></button>
      <button @click="toggleEmojiPanel" class="emoji-button"></button>
      
      <div v-if="showEmojiPanel" class="emoji-panel">
       <div class="emoji">
        <span @click="addEmoji('😊')">😊</span>
        <span @click="addEmoji('😂')">😂</span>
        <span @click="addEmoji('😍')">😍</span>
        <span @click="addEmoji('😎')">😎</span>
        <span @click="addEmoji('😢')">😢</span>
        <span @click="addEmoji('👍')">👍</span>
       </div> 
        
      </div>
    </div>
   
    </div>

    
</template>

<script>
export default {
  name: "ChatComponent",
  data() {
    return {
      newMessage: "",
      messages: [],
      username: "User" + Math.floor(Math.random() * 100),
      showEmojiPanel: false,
    };
  },
  methods: {
    sendMessage() {
      if (this.newMessage.trim() !== "") {
        const timeNow = new Date().toLocaleTimeString([], { hour: "2-digit", minute: "2-digit" });

        this.messages.push({
          user: this.username,
          text: this.newMessage,
          time: timeNow,
        });

        this.newMessage = "";
        this.scrollToBottom();
      }
    },
    scrollToBottom() {
      this.$nextTick(() => {
        const container = this.$refs.messageContainer;
        if (container) container.scrollTop = container.scrollHeight;
      });
    },
    toggleEmojiPanel() {
      this.showEmojiPanel = !this.showEmojiPanel;
    },
    addEmoji(emoji) {
      this.newMessage += emoji;
      this.showEmojiPanel = false; // Закрыть панель после выбора эмодзи
    }
  }
};
</script>

<style scoped>
.title {
  text-align: center;
  color: #fff;
}
.title img {
  position: relative;
  height: 20px;
  width: 20px;
}
.chat {
  max-width: 600px;
  margin: auto;
  padding: 20px;
  border-radius: 5px;
  height: 500px;
  box-shadow: 5px 5px 20px rgba(255, 255, 255, 0.3), -5px -5px 20px rgba(255, 255, 255, 0.3);
}

.user-name {
  background: transparent;
  font-size: 12px;
  color: #ddff00;
}

.user-text {
  background: transparent;
}

.messages {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  height: 430px;
  overflow-y: auto;
  margin-bottom: 10px;
  padding-right: 10px;
  scrollbar-width: thin;
  scrollbar-color: rgba(255, 255, 255, 0.5) transparent;
}

.messages .my-message {
  align-self: flex-end;
  margin-top: 10px;
  background: #34357a;
  color: white;
  text-align: right;
}

.messages .other-message {
  align-self: flex-start;
  background: #ededed;
  color: rgb(53, 94, 0);
}

.messages::-webkit-scrollbar {
  width: 8px !important;
}

.messages::-webkit-scrollbar-thumb {
  background-color: rgba(9, 30, 221, 0.675) !important;
  border-radius: 10px !important;
}

.messages::-webkit-scrollbar-track {
  background: transparent !important;
}

input {
  flex: 1;
  padding: 10px;
  font-size: 15px;
  border: 1px solid #ffffff;
  border-radius: 5px;
}

input::placeholder {
  font-size: 15px;
  color: #ffffff56;
}

.message {
  display: block;
  width: fit-content;
  padding: 10px;
  border-radius: 8px;
  background: transparent;
  color: #fff;
  word-wrap: break-word;
  overflow-wrap: break-word;
  max-width: 70%;
}

.time {
  display: flex;
  margin-top: 10px;
  font-size: 12px;
  color: #eaff00;
  justify-content: flex-end;
  right: 10px;
  background: transparent;
}

input {
  width: 80%;
  padding: 10px;
  background: transparent;
  color: #fff;
}

button {
  margin-top: 20px;
  padding: 5px;
  height: 25px;
  width: 25px;
  margin-left: 20px;
  background: transparent;
  background-image: url("../assets/send.png");
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  border: none;
  cursor: pointer;
}

/* Для кнопки смайлика */
.emoji-button {
  background-image: url("../assets/smile.png");
  height: 25px;
  width: 25px;
  background-size: contain;
  background-repeat: no-repeat;
  cursor: pointer;
  position: relative; /* Это нужно для точного позиционирования */
}

/* Панель смайликов */
.emoji-panel {
  position: absolute; /* Позиционирование относительно родительского контейнера */
  bottom: 40px; /* Расстояние между панелью и кнопкой смайликов */
  right: 0; /* Позиционируем по правому краю */
  background-color: #ffffff08; /* Полупрозрачный фон */
  padding: 10px;
  border-radius: 5px;
  display: flex;
  flex-direction: column; /* Размещаем смайлики вертикально */
  align-items: flex-start; /* Выравниваем смайлики по левому краю */
  gap: 10px;
  z-index: 999; /* Панель будет поверх других элементов */
}

.emoji-panel span {
  font-size: 20px;
  cursor: pointer;
}
</style>