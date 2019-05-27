<template>
  <div class="sc-user-input--picker-wrapper">
      <div
        tabIndex="0"
        v-if="isActive"
        @blur="_handlePickerBlur"
        class="sc-quickreply-picker">
        <div class="sc-quickreply-picker--content">
            <div class="sc-quickreply-picker--category">
                <div class="sc-quickreply-picker--category-title">Quick Reply</div>
                <span v-for="(qr, index) in quickReplyList" :key="index">
                    <el-tooltip class="item" effect="dark" :open-delay="500" :content="qr.quickReply" placement="top">
                        <el-button @click="onQuickReplyPicked(qr.quickReply), isActive = false" style="margin: 10px 0 0 10px" size="mini">{{ qr.title }}</el-button>
                    </el-tooltip>
                </span>
            </div>
            <div class="sc-quickreply-picker--category" v-if="quickReplyList.length == 0">
                You do not have quick reply.
            </div>
        </div>
    </div>
    <el-button type="text" @click.prevent="_openPicker" style="font-size: 18px; color: rgb(86, 88, 103)"><i class="mdi mdi-comment-multiple-outline" /></el-button>
    </div>
</template>

<script>

export default {
  components: {
    
  },
  data () {
    return {
      isActive: false
    }
  },
  props: {
    onQuickReplyPicked: {
      type: Function,
      required: true
    },
    quickReplyList: {
        type: Array,
        required: false,
        default: () => []
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
.sc-quickreply-picker {
  position: absolute;
  bottom: 50px;
  right: 0px;
  width: 330px;
  max-height: 215px;
  box-shadow: 0px 7px 40px 2px rgba(148, 149, 150, 0.3);
  background: white;
  border-radius: 10px;
  outline: none;
}

.sc-quickreply-picker:after {
  content: "";
  width: 14px;
  height: 14px;
  background: white;
  position: absolute;
  bottom: -6px;
  right: 80px;
  transform: rotate(45deg);
  border-radius: 2px;
}

.sc-quickreply-picker--content {
  padding: 10px;
  overflow: auto;
  width: 100%;
  max-height: 195px;
  margin-top: 7px;
  box-sizing: border-box;
}

.sc-quickreply-picker--category {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.sc-quickreply-picker--category-title {
  min-width: 100%;
  color: #b8c3ca;
  font-weight: 200;
  font-size: 13px;
  margin: 5px;
  letter-spacing: 1px;
}

.sc-quickreply-picker--emoji {
  margin: 5px;
  width: 30px;
  line-height: 30px;
  text-align: center;
  cursor: pointer;
  vertical-align: middle;
  font-size: 28px;
  transition: transform 60ms ease-out,-webkit-transform 60ms ease-out;
}

.sc-quickreply-picker--emoji:hover {
  transform: scale(1.4);
}
</style>