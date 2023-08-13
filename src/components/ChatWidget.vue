<template>
  <div class="chat-widget">

    <div class="chat-messages">

      <div v-for="msg in messages" :key="msg.id" class="message"
        :class="{ 'from-user': msg.from === 'user', 'from-bot': msg.from === 'bot' }">

        <div class="avatar"></div>

        <div v-if="msg.id === 'typing'" class="loading">
          <!-- <span class="loading-icon"></span> -->
              <div class="typingIndicatorBubbleDot"></div>
              <div class="typingIndicatorBubbleDot"></div>
              <div class="typingIndicatorBubbleDot"></div>
        </div>
        {{ msg.text }}
      </div>

    </div>

    <div class="chat-input">
      <input v-model="input" @keyup.enter="sendMessage" placeholder="Напишите сообщение" />
    </div>

    <div class="buttons">
      <button v-for="button in buttons" @click="sendButtonMessage(button.text)">
        {{ button.text }}
      </button>
    </div>

  </div>
</template>
<script>
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
        { text: 'Еще кнопка с текстом' }
      ],
      isLoading: false
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
        } else {
          this.messages.push({
            id: this.messages.length + 1,
            text: 'К сожалению, я Вас не понял. Попробуйте задать вопрос другими словами',
            from: 'bot'
          })
        }
      }, 1000)
    }
  }
}
</script>

<style>
.chat-widget {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  height: 100vh;
  max-height: 800px;
  border-radius: 10px;
  background-color: #ffffff;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
}

.message {
  display: flex;
  margin-bottom: 10px;
  background-color: #f0f0f0;
  border-radius: 10px;
  padding: 10px;
  align-items: flex-end;
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
}

.chat-input input {
  width: calc(100% - 40px);
  height: 40px;
  border: none;
  outline: none;
  border-radius: 10px;
  padding: 0 10px;
  margin: 10px 0;
  background-color: #f0f0f0;
}

.chat-input input:hover {
  background-color: #ccc;
}

.buttons {
  display: flex;
  padding: 10px;
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

.typingIndicatorBubbleDot {
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

.typingIndicatorBubbleDot:first-of-type {
  margin: 0px 4px;
}

.typingIndicatorBubbleDot:nth-of-type(2) {
  animation-delay: 0.15s;
}

.typingIndicatorBubbleDot:nth-of-type(3) {
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