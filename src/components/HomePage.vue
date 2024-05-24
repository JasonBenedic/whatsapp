<template>
  <div id="app">
    <div class="top-bar">
      <img
        src="@/assets/LogoWa.png"
        alt="WhatsApp Logo"
        style="
          height: 35px;
          width: 35px;
          padding-bottom: 5px;
          margin-right: 10px;
        "
      />
      <h1 class="whatsapp-title">WhatsApp</h1>
    </div>
    <div class="home">
      <div class="icon-sidebar">
        <img
          src="@/assets/message.png"
          style="height: 35px; margin-top: 5px; cursor: pointer"
          @click="showChatWindow"
        />
        <img
          src="@/assets/call.png"
          style="height: 35px; margin-top: 10px; cursor: pointer"
          @click="hideChatWindow"
        />
        <img
          src="@/assets/status.png"
          style="height: 35px; margin-top: 10px"
          @click="doNothing"
        />
      </div>
      <div class="sidebar">
        <div class="sidebar-header">
          <h4>WhatsApp Web</h4>
          <input
            type="text"
            class="form-control"
            placeholder="Search or start new chat"
            v-model="search"
          />
        </div>
        <div class="contact-list">
          <div
            v-for="contact in filteredContacts"
            :key="contact.id"
            v-show="contact.id !== 1 || chatVisible"
            class="contact"
            @click="selectContact(contact)"
          >
            <img
              src="@/assets/PP.png"
              alt="avatar"
              style="
                background-color: white;
                border-radius: 25px;
                height: 45px;
                margin-right: 7px;
              "
            />
            <div class="contact-info">
              <h7>{{ contact.name }}</h7>
              <h9>{{ contact.lastMessage }}</h9>
            </div>
          </div>
        </div>
      </div>
      <div
        v-if="chatVisible"
        class="chat-window"
        style="background-image: url('@/assets/BG.png')"
      >
        <div v-if="selectedContact" class="chat-header">
          <h5>{{ selectedContact.name }}</h5>
        </div>
        <div v-if="selectedContact" class="chat-window-content">
          <div class="messages" ref="messages">
            <div
              v-for="message in messages"
              :key="message.id"
              :class="['message', message.sender]"
            >
              <p>{{ message.text }}</p>
            </div>
          </div>
          <div class="chat-input">
            <input
              type="text"
              v-model="newMessage"
              @keypress.enter="sendMessage"
              class="form-control"
              placeholder="Type a message"
            />
          </div>
        </div>
        <div v-else class="no-chat-selected">
          <p>Select a contact to start chatting</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      search: "",
      selectedContact: null,
      newMessage: "",
      contacts: [
        {
          id: 1,
          name: "SD2 Only Ba,Dev",
          lastMessage: "Hello!",
        },
        {
          id: 2,
          name: "Haryadi SD2",
          lastMessage: "How are you?",
        },
        {
          id: 3,
          name: "Daud SD2",
          lastMessage: "How are you?",
        },
        // Add more contacts here
      ],
      messages: [
        { id: 1, sender: "received", text: "Hello!" },
        { id: 2, sender: "sent", text: "Hi, how are you?" },
        // Add more messages here
      ],
      chatVisible: true,
    };
  },
  computed: {
    filteredContacts() {
      return this.contacts.filter((contact) =>
        contact.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
  },
  created() {
    if (this.contacts.length > 0) {
      this.selectedContact = this.contacts[0];
    }
  },
  methods: {
    showChatWindow() {
      this.chatVisible = true;
    },
    hideChatWindow() {
      this.chatVisible = false;
    },
    doNothing() {
      // Do nothing when the third button is clicked
    },
    selectContact(contact) {
      this.selectedContact = contact;
    },
    sendMessage() {
      if (this.newMessage.trim() !== "") {
        this.messages.push({
          id: Date.now(),
          sender: "sent",
          text: this.newMessage,
        });
        this.newMessage = "";
        this.$nextTick(() => {
          this.$refs.messages.scrollTop = this.$refs.messages.scrollHeight;
        });
      }
    },
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  overflow: hidden;
}
html {
  margin: 0;
  padding: 0;
  overflow: hidden;
}
#app {
  margin: 0;
  padding: 0;
}

.top-bar {
  display: flex;
  align-items: center;
  background-color: #333;
  color: white;
  padding: 11.5px 20px;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
}

.whatsapp-logo {
  width: 40px; /* Adjust size as needed */
  height: 40px; /* Adjust size as needed */
  margin-right: 10px;
}

.whatsapp-title {
  font-size: 1.5rem;
  font-weight: bold;
}

.home {
  display: flex;
  height: 100vh;
}

.icon-sidebar {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  width: 50px;
  background: #333;
  color: white;
  margin-top: 60px;
}

.icon-sidebar a {
  color: #fff;
  text-decoration: none;
  margin: 10px 0;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 40px;
  width: 40px;
}

.sidebar {
  margin-top: 20px;
  width: 30%;
  height: 100%;
  background: #1e1e1e;
  border-right: 1px solid #333;
}

.sidebar-header {
  padding: 15px;
  border-bottom: 1px solid #333;
}

.contact-list {
  overflow-y: auto;
  height: calc(100vh - 60px);
}

.contact {
  display: flex;
  align-items: center;
  padding: 10px;
  cursor: pointer;
  border-bottom: 1px solid #333;
  color: white;
}

.contact:hover {
  background: #333;
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 10px;
}

.contact-info {
  display: flex;
  flex-direction: column;
}

.chat-window {
  flex-grow: 1;
  height: 100%; /* Adjust height as needed */
  display: flex;
  flex-direction: column;
  /* background: #1e1e1e; */
  color: #e0e0e0;
  background: url("/src/assets/BG.png");
  background-color: black;
  background-size: 40%;
}

.chat-window-content {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
}

.messages {
  flex-grow: 1;
  overflow-y: auto;
  padding: 15px;
}

.chat-input {
  padding: 15px;
  border-top: 1px solid #333;
  background: #1e1e1e;
  margin-top: auto; /* Push chat input to the bottom */
}

.no-chat-selected {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}

.chat-header {
  background-color: #e0e0e0; /* Add background color to chat header */
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

.messages {
  flex-grow: 1;
  overflow-y: auto;
  padding: 15px;
}

.message {
  margin-bottom: 10px;
  max-width: 70%;
}

.message.sent {
  align-self: flex-end;
  background: #056162;
  padding: 10px;
  border-radius: 10px;
  color: #e0e0e0;
}

.message.received {
  align-self: flex-start;
  background: #2a2a2a;
  padding: 10px;
  border-radius: 10px;
  color: #e0e0e0;
}

.chat-input {
  padding: 15px;
  border-top: 1px solid #333;
  background: #1e1e1e;
}

.no-chat-selected {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}
</style>
