<template>
  <div class="sc-user-input--picker-wrapper">
      <EmojiPicker 
        v-if="isActive"
        :onEmojiPicked="onEmojiPicked"
        :onBlur="_handlePickerBlur"
      />
      <el-button type="text" @click.prevent="_openPicker" style="font-size: 18px; color: rgb(86, 88, 103)"><i class="mdi mdi-emoticon-happy" /></el-button>
      </div>
</template>

<script>
import EmojiPicker from './EmojiPicker.vue'

export default {
  components: {
    EmojiPicker
  },
  data () {
    return {
      isActive: false
    }
  },
  props: {
    onEmojiPicked: {
      type: Function,
      required: true
    },
    color: {
      type: String,
      required: true
    }
  },
  methods: {
    _openPicker (e) {
      this.isActive = !this.isActive
    },
    _handlePickerBlur () {
      this.isActive = false
    }
  }
}
</script>

<style scoped>
.sc-user-input--emoji-icon-wrapper {
  background: none;
  border: none;
  padding: 0px;
  margin: 0px;
  outline: none;
}

.sc-user-input--emoji-icon-wrapper:focus {
  outline: none;
}

.sc-user-input--emoji-icon {
  height: 18px;
  cursor: pointer;
  align-self: center;
}

.sc-user-input--emoji-icon-wrapper:focus .sc-user-input--emoji-icon path,
.sc-user-input--emoji-icon-wrapper:focus .sc-user-input--emoji-icon circle,
.sc-user-input--emoji-icon.active path,
.sc-user-input--emoji-icon.active circle,
.sc-user-input--emoji-icon:hover path,
.sc-user-input--emoji-icon:hover circle {
  filter: contrast(15%);
}
</style>
