<template>
  <div class='sc-message--file' :style="messageColors" v-loading="data.proccessUpload ? data.proccessUpload : false">
    <div class='sc-message--file-icon'>
      <!-- <a :href="data.file.url || '#'" target='_blank'>
        <img src="./assets/file.svg" alt='generic file icon' height="60" />
      </a> -->
    </div>
    <div class='sc-message--file-name' :style="messageColors">
       <a v-if="imageFile.indexOf(checkFileType(data.file.src)) !== -1" v-on:click="data.file.onclick" target="_blank"><img :src="data.file.src" class="sc-image"/></a>
       <a class="linkwrap" v-else-if="videoFile.indexOf(checkFileType(data.file.src)) !== -1" v-on:click="data.file.onclick" target="_blank">
        <div class="blocker"></div> 
        <iframe :src="data.file.src" v-on:click="data.file.onclick"/>
       </a>
       <a v-else v-on:click="data.file.onclick" target="_blank">
         <img src="./assets/file.svg" height="60" :alt="data.file.src" />
       </a>
    </div>
    <div class="sc-message--file-text" :style="messageColors">
      <span v-if="imageFile.indexOf(checkFileType(data.file.src)) !== -1">{{data.text}}</span>
      <span v-else style="color: gray; font-weight: 700">{{data.text}}</span>
      <p v-if="data.meta" class='sc-message--meta' :style="messageColors">{{data.meta}}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    data: {
      type: Object,
      required: true
    },
    messageColors: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      imageFile: ['png', 'jpg', 'gif', 'jpeg'],
      videoFile: ['mp4', 'mkv', 'mov']
    }
  },
  methods:{
    checkFileType(data){
      if(data.substring(0, 4) == "http"){
        let reverseStr = this.reverse(data)
        return this.reverse(reverseStr.substring(0, reverseStr.indexOf(".")))
      } else {
        return data.substring("data:image/".length, data.indexOf(";base64"))
      }
    },
    reverse(string){
      return string.split("").reverse().join("")
    }
  }
}
</script>

<style scoped>
.linkwrap { position:relative; display:inline-block; }
.blocker { position:absolute; height:100%; width:100%; z-index:1; background:transparent;  }
.linkwrap iframe { z-index: 2; }

.sc-message--file {
  border-radius: 6px;
  font-weight: 300;
  font-size: 14px;
  line-height: 1.4;
  /* white-space: pre-wrap; */
  -webkit-font-smoothing: subpixel-antialiased
}
.sc-image {
  max-width: 100%;
  min-width: 100%;
}
.sc-message--content.sent .sc-message--file {
  word-wrap: break-word;
}

.sc-message--file-icon {
  text-align: center;
  margin-left: auto;
  margin-right: auto;
  margin-top: 15px;
  margin-bottom: 0px;
}

.sc-message--file-icon:hover {
  opacity: 0.7;
}

.sc-message--file-text {
  padding: 17px 20px;
  border-radius: 6px;
  font-weight: 300;
  font-size: 14px;
  line-height: 1.4;
  white-space: pre-wrap;
  -webkit-font-smoothing: subpixel-antialiased
}

.sc-message--file-name {
  color: white;
  padding-left: 15px;
  padding-right: 15px;
  padding-top: 0;
  font-size: x-small;
  text-align: center;
}

.sc-message--file-name a {
  text-decoration: none;
  color: #ece7e7;
}

.sc-message--file-name a:hover {
  color: white;
}

.sc-message--content.sent .sc-message--file-text {
  color: white;
  background-color: #4e8cff;
  word-wrap: break-word;
}

.sc-message--content.received .sc-message--file {
  color: #263238;
  background-color: #f4f7f9;
  margin-right: 40px;
}

.sc-message--content.received .sc-message--file-name {
  color: #000;
}

.sc-message--content.received .sc-message--file a {
  color: rgba(43, 40, 40, 0.7);
}

.sc-message--content.received .sc-message--file a:hover {
  color: #0c0c0c;
}
</style>
