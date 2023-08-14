<template>
  <div class="chat-widget">

    <div class="chat-messages">

      <div v-for="msg in messages" :key="msg.id" class="message"
        :class="{ 'from-user': msg.from === 'user', 'from-bot': msg.from === 'bot' }">
        <div class="avatar"></div>

        <div class="text">
          <div v-if="msg.id === 'typing'" class="loading">
            <!-- <span class="loading-icon"></span> -->
            <div class="typing-indicator-bubble-dot"></div>
            <div class="typing-indicator-bubble-dot"></div>
            <div class="typing-indicator-bubble-dot"></div>
          </div>
          {{ msg.text }}
        </div>
      </div>
    </div>
    <div class="chat-input">
      
      <button class="menu-button" @click="toggleMenu"></button>
      <input v-model="input" @keyup.enter="sendMessage" placeholder="Напишите сообщение" />
      <!-- <button class="send-button" @click="sendMessage"></button> -->
      <button v-if="showSendButton" :class="{ 'show': showSendButton }" class="send-button" @click="sendMessage"></button>
    </div>
    

    <div class="buttons" v-if="showMenu">
      <button v-for="button in buttons" @click="sendButtonMessage(button.text)">
        {{ button.text }}
      </button>
    </div>

  </div>
</template>
<script>
import { nextTick } from 'vue'

export default {
  data() {
    return {
      messages: [],
      input: '',
      buttons: [
        { text: 'Заказать пиццу' },
        { text: 'Установить будильник' },
        { text: 'Показать погоду' },
        { text: 'Кнопка с текстом' },
        { text: 'Еще кнопка с текстом' },
        { text: 'Lorem ipsum' },
      ],
      isLoading: false,
      showMenu: false,
    }
  },

  mounted() {
    this.messages.push({
      id: 1,
      text: 'Привет! Чем я могу Вам помочь?',
      from: 'bot'
    })
  },

  methods: {
    addMessage(msg) {
      this.messages.push(msg)

    },
    toggleMenu() {
      this.showMenu = !this.showMenu
    },
    sendMessage() {
      if (this.input) {
        this.messages.push({
          id: this.messages.length + 1,
          text: this.input,
          from: 'user'
        })
        this.input = ''

        this.reply()
      }
    },

    sendButtonMessage(text) {
      this.messages.push({
        text,
        from: 'user'
      })

      this.input = ''

      this.reply()
    },

    reply() {
      const lastMsg = this.messages[this.messages.length - 1]

      const botTyping = {
        id: 'typing',
        from: 'bot'
      }

      this.messages.push(botTyping)


      setTimeout(() => {
        this.messages = this.messages.filter(msg => msg.id !== 'typing')
        if (lastMsg.text.includes('пицц')) {
          this.messages.push({
            id: this.messages.length + 1,
            text: 'Хорошо, я закажу Вам пиццу!',
            from: 'bot'
          })
        } else if (lastMsg.text.includes('будильник')) {
          this.messages.push({
            id: this.messages.length + 1,
            text: 'Я установил для Вас будильник',
            from: 'bot'
          })
        } else if (lastMsg.text.includes('погоду')) {
          this.messages.push({
            id: this.messages.length + 1,
            text: 'Я отправил Вам погоду',
            from: 'bot'
          })
        } else if (lastMsg.text.includes('Lorem')) {
          this.messages.push({
            id: this.messages.length + 1,
            text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
            from: 'bot'
          })
        } else {
          this.messages.push({
            id: this.messages.length + 1,
            text: 'К сожалению, я Вас не понял. Попробуйте задать вопрос другими словами',
            from: 'bot'
          })
        }
      }, (Math.random(5) * 1000))
    }
  },

  computed: {

    showSendButton() {
      return this.input.length > 0;
    },
    

  }

}

</script>

<style>
.chat-widget {
  width: 500px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  height: 750px;
  border-radius: 10px;
  background-color: #ffffff;
  box-shadow: 20px 20px 40px rgba(0, 0, 0, 0.1);
}

.chat-messages {
  flex: 1;
  padding: 10px;
  height: 400px;
  overflow: overlay;
}

.chat-messages::-webkit-scrollbar {
  width: 5px;
  height: 100%;
}

/* .chat-messages::-webkit-scrollbar-track {
  background: #f0f0f0;
} */

.chat-messages::-webkit-scrollbar-thumb {
  background: #ccc;
  border-radius: 5px;
}

.message {
  display: flex;
  margin-bottom: 10px;
  /* background-color: #f0f0f0; */
  /* border-radius: 10px; */
  /* padding: 10px; */
  align-items: flex-end;
}

.text {
  display: flex;
  background-color: #f0f0f0;
  border-radius: 10px;
  width: fit-content;
  max-width: calc(100% - 120px);
  padding: 10px;
  height: 100%;
  text-align: start;
}

.from-user {
  justify-content: flex-end;
}

.from-bot {
  justify-content: flex-start;
}

.message .avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin: 0;
  flex-shrink: 0;
}

.from-user .avatar {
  order: 1;
  background-image: url(../assets/user.png);
  background-position: center;
  background-size: contain;
  margin-left: 10px;
}

.from-bot .avatar {
  order: 0;
  background-image: url(../assets/bot.png);
  background-position: center;
  background-size: contain;
  margin-right: 10px;
}

.chat-input {
  border-top: 1px solid #ccc;
  padding: 0;
  display: flex;
  align-items: center;
}

.chat-input input {
  width: 100%;
  height: 40px;
  border: none;
  outline: none;
  border-radius: 10px;
  padding: 0 10px;
  margin: 10px;
  background-color: #f0f0f0;
}

.chat-input input:hover {
  background-color: #ccc;
}

.send-button {
  transform: translateX(100%);
  width: 30px;
  height: 100%;
  background-image: url(../assets/send-message.png);
  background-position: center;
  background-size: contain;
  background-color: unset;
  background-repeat: no-repeat;
  /* padding: 0; */
  border: none;
  outline: none;
  margin: 10px 10px 10px 0;
  cursor: pointer;
}

.menu-button {
  width: 30px;
  height: 30px;
  background-image: url(../assets/menu.png);
  background-position: center;
  background-size: contain;
  background-color: unset;
  background-repeat: no-repeat;
  /* padding: 0; */
  border: none;
  outline: none;
  margin: 10px 0 10px 10px;
  cursor: pointer;
}

.show {
  transform: translateX(0);
}

.send-button:hover {
  opacity: 0.8;
}

.buttons {
  display: flex;
  padding: 0 10px 10px 10px;
  overflow-x: auto;
  gap: 10px;
  flex-wrap: wrap;
  justify-content: space-between;
}

.buttons::-webkit-scrollbar {
  display: none;
}

.buttons button {
  padding: 10px 20px;
  border: none;
  background: #007bff;
  color: #fff;
  border-radius: 10px;
  cursor: pointer;
  width: max-content;
  flex-grow: 1;
  font-family: inherit;
  font-weight: 700;
}

.buttons button:hover {
  background: #0069d9;
}

/* Стили прокрутки */
.buttons::-webkit-scrollbar {
  height: 5px;
}

.buttons::-webkit-scrollbar-thumb {
  background: #ccc;
  border-radius: 5px;
}

.loading {
  display: flex;
  align-items: flex-end;
  height: 100%;
}

/* .loading-icon {
  width: 30px;
  height: 30px;
  border: 3px solid #ccc;
  border-right-color: #007bff;
  border-radius: 50%;

  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
} */

.typing-indicator-bubble-dot {
  width: 4px;
  height: 4px;
  margin-right: 4px;
  background-color: #57585a;
  border-radius: 50%;
  animation-name: bounce;
  animation-duration: 1.3s;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
}

.typing-indicator-bubble-dot:first-of-type {
  margin: 0px 4px;
}

.typing-indicator-bubble-dot:nth-of-type(2) {
  animation-delay: 0.15s;
}

.typing-indicator-bubble-dot:nth-of-type(3) {
  animation-delay: 0.3s;
}

@keyframes bounce {

  0%,
  60%,
  100% {
    transform: translateY(0);
  }

  30% {
    transform: translateY(-4px);
  }
}
</style>