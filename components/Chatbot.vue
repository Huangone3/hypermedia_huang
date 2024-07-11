<template>
  <div>
    <button class="chatbot-button" @click="toggleChatbot">Chat</button>
    <div v-if="isChatOpen" class="chatbot-popup">
      <div class="chat-header">
        <h2>BestBot</h2>
        <button @click="toggleChatbot">X</button>
      </div>
      <div class="chat-window">
        <div v-for="(message, index) in messages" :key="index" class="message" :class="message.sender">
          <span class="name">{{ message.sender === 'user' ? 'User' : 'BestBot' }}</span>
          <div class="message-content">{{ message.text }}</div>
        </div>
      </div>
      <div class="input-area">
        <input v-model="userInput" @keyup.enter="sendMessage" placeholder="Type your message here..." />
        <button @click="sendMessage">Send</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isChatOpen: false,
      messages: [],
      userInput: ''
    };
  },
  mounted() {
    // Load message from localStorage
    const savedMessages = localStorage.getItem('chatbotMessages');
    if (savedMessages) {
      this.messages = JSON.parse(savedMessages);
    }
  },
  methods: {
    toggleChatbot() {
      this.isChatOpen = !this.isChatOpen;
      console.log('Chatbot visibility toggled:', this.isChatOpen);

      // Show welcome message if first message
      if (this.isChatOpen && this.messages.length === 0) {
        this.messages.push({
          text: 'Welcome, I am BestBot, your personal assistant. If you have any problems, feel free to ask me!',
          sender: 'bot'
        });
        // Save message to localStorage
        this.saveMessagesToLocalStorage();
      }
    },
    async sendMessage() {
      if (this.userInput.trim() === '') return;

      const userMessage = {
        text: this.userInput,
        sender: 'user'
      };

      this.messages.push(userMessage);
      this.userInput = '';

      // Save updated messages to localStorage
      this.saveMessagesToLocalStorage();

      try {
        // Send message to the server-side API
        const response = await fetch('/api/openai', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ message: userMessage.text })
        });

        if (!response.ok) {
          throw new Error('Failed to get response from server');
        }

        const data = await response.json();
        this.messages.push({
          text: data.message,
          sender: 'bot'
        });

        // Save updated messages to localStorage
        this.saveMessagesToLocalStorage();
      } catch (error) {
        console.error('Error sending message to OpenAI:', error);
        this.messages.push({
          text: 'Sorry, there was an error processing your request.',
          sender: 'bot'
        });

        // Save updated error message to localStorage
        this.saveMessagesToLocalStorage();
      }
    },
    saveMessagesToLocalStorage() {
      localStorage.setItem('chatbotMessages', JSON.stringify(this.messages));
    }
  }
};
</script>

<style scoped>
.chatbot-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 50%;
  width: 60px;
  height: 60px;
  font-size: 14px;
  cursor: pointer;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  text-align: center;
}

.chatbot-popup {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 390px;
  height: 520px;
  background-color: white;
  border: 1px solid #ddd;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  z-index: 1000;
}

.chat-header {
  background-color: #007bff;
  color: white;
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.chat-window {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
}

.message {
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 5px;
  max-width: 80%;
  word-wrap: break-word;
  position: relative;
}

.message.user {
  background-color: #d6eaff;
  margin-left: auto;
  margin-right: 10px;
}

.message.bot {
  background-color: #e0e0e0;
  margin-right: auto;
  margin-left: 10px;
}

.message .name {
  font-weight: bold;
  display: block;
  margin-bottom: 5px;
}

.message-content {
  font-size: 20px;
}

.input-area {
  display: flex;
  padding: 10px;
  border-top: 1px solid #ddd;
}

input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 3px;
  box-sizing: border-box;
  max-width: 100%;
  overflow: hidden;
}

button {
  margin-left: 10px;
  padding: 10px 20px;
  border: none;
  border-radius: 3px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
}
</style>