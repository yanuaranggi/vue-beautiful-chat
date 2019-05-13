<template>
  <div class="sc-chat-window" :class="{opened: isOpen, closed: !isOpen}" v-loading.sync="loadingWindow">
    <Header
      :title="title"
      :imageUrl="titleImageUrl"
      :onClose="onClose"
      :colors="colors"
      @userList="handleUserListToggle"
      :onMessageButton="onMessageButton"
      :onGroupButton="onGroupButton"
      :showGroupButton="showGroupButton"
    />
    <UserList 
      v-if="showUserList"
      :participants="participants"
      :detail="ticketDetail"
    />
    <MessageList
      v-if="!showUserList"
      :messages="messages"
      :participants="participants"
      :showTypingIndicator="showTypingIndicator"
      :colors="colors"
      :alwaysScrollToBottom="alwaysScrollToBottom"
      :messageStyling="messageStyling"
    />
    <UserInput
      v-if="showInput && !showBtnReach"
      :showEmoji="showEmoji"
      :onSubmit="onUserInputSubmit"
      :suggestions="getSuggestions()"
      :showFile="showFile"
      :placeholder="placeholder"
      :colors="colors" />
    <el-button-group>
      <el-button @click="onClickButtonReach" style="border-radius: 0 0 0 4px; width: 65%;" type="primary" v-if="showBtnReach">Reach out</el-button>
      <el-button @click="onClickButtonClose" style="border-radius: 0 0 4px 0; width: 35%;" type="danger" :loading="loadingButtonClose" v-if="showButtonClose">Close</el-button>
    </el-button-group>
  </div>
</template>

<script>
import Header from './Header.vue'
import MessageList from './MessageList.vue'
import UserInput from './UserInput.vue'
import UserList from './UserList.vue'

export default {
  components: {
    Header,
    MessageList,
    UserInput,
    UserList
  },
  props: {
    ticketDetail: {
      type: Object,
      required: false,
    },
    showEmoji: {
      type: Boolean,
      default: false
    },
    showInput: {
      type: Boolean,
      default: true
    },
    showButtonReach: {
      type: Boolean,
      default: false,
      required: true
    },
    showButtonClose: {
      type: Boolean,
      default: false,
      required: true
    },
    loadingButtonClose: {
      type: Boolean,
      default: false,
      required: false
    },
    onClickButtonReach: {
      type: Function,
      required: false
    },
    onClickButtonClose: {
      type: Function,
      required: false
    },
    showFile: {
      type: Boolean,
      default: false
    },
    participants: {
      type: Array,
      required: true
    },
    title: {
      type: String,
      required: true
    },
    titleImageUrl: {
      type: String,
      default: ''
    },
    onUserInputSubmit: {
      type: Function,
      required: true
    },
    onClose: {
      type: Function,
      required: true
    },
    onMessageButton: {
      type: Function,
      required: false,
      default: () => {}
    },
    showGroupButton:{
      type: Boolean,
      required: true,
      default: false
    },
    onGroupButton: {
      type: Function,
      required: false,
      default: () => {}
    },
    messageList: {
      type: Array,
      default: () => []
    },
    isOpen: {
      type: Boolean,
      default: () => false
    },
    placeholder: {
      type: String,
      default: 'Write a reply'
    },
    showTypingIndicator: {
      type: String,
      required: true
    },
    colors: {
      type: Object,
      required: true
    },
    alwaysScrollToBottom: {
      type: Boolean,
      required: true
    },
    messageStyling: {
      type: Boolean,
      required: true
    },
    loadingMessage: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      showUserList: false,
      loadingWindow: false,
      showBtnReach: false
    }
  },
  computed: {
    messages() {
      let messages = this.messageList

      return messages
    }
  },
  methods: {
    handleUserListToggle(showUserList) {
      this.showUserList = showUserList
    },
    getSuggestions(){
      return this.messages.length > 0 ? this.messages[this.messages.length - 1].suggestions : []
    }
  },
  watch: {
    loadingMessage: function(){
      setTimeout(() => {
        this.loadingWindow = this.loadingMessage
      }, 100)
    },
    showButtonReach: function(){
      setTimeout(() => {
        this.showBtnReach = this.showButtonReach
      }, 100)
    }
  }
}
</script>
<style scoped>
.sc-chat-window {
  width: 370px;
  height: calc(100% - 120px);
  max-height: 590px;
  position: fixed;
  right: 25px;
  bottom: 100px;
  box-sizing: border-box;
  box-shadow: 0px 7px 40px 2px rgba(148, 149, 150, 0.1);
  background: white;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: 0.3s ease-in-out;
  border-radius: 10px;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

.sc-chat-window.closed {
  opacity: 0;
  visibility: hidden;
  bottom: 90px;
}

.sc-message--me {
  text-align: right;
}
.sc-message--them {
  text-align: left;
}

@media (max-width: 450px) {
  .sc-chat-window {
    width: 100%;
    height: 100%;
    max-height: 100%;
    right: 0px;
    bottom: 0px;
    border-radius: 0px;
  }
  .sc-chat-window {
    transition: 0.1s ease-in-out;
  }
  .sc-chat-window.closed {
    bottom: 0px;
  }
}
</style>
