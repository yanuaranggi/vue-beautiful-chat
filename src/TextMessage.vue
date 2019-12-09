<template>
  <div class="sc-message--text" :style="messageColors" v-loading="data.proccessUpload ? data.proccessUpload : false">
    <p v-html="messageText"></p>
    <p v-if="data.meta" class='sc-message--meta' :style="{color: messageColors.color}">{{data.meta}}</p>
  </div>
</template>

<script>
import escapeGoat from 'escape-goat'
import Autolinker from 'autolinker'
import * as EmailValidator from 'email-validator';

const fmt = require('msgdown')

export default {
  props: {
    data: {
      type: Object,
      required: true
    },
    messageColors: {
      type: Object,
      required: true
    },
    messageStyling: {
      type: Boolean,
      required: true
    }
  },
  computed: {
    messageText() {
      const defaultTokens = {
        bold: { delimiter: '*', tag: 'strong' },
        italic: { delimiter: '_', tag: 'em' },
        underline: { delimiter: '%', tag: 'u' },
        strike: { delimiter: '~', tag: 'del' },
        code: { delimiter: '`', tag: 'code' },
        sup: { delimiter: '^', tag: 'sup' },
        sub: { delimiter: '¡', tag: 'sub' }
      }
      let emailRegex = /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$/g
      const escaped = escapeGoat.escape(this.data.text)
      let escapedArr = escaped.split(/\s/g)
      let joinStr = escapedArr.map(v => {
        if(EmailValidator.validate(v)){
          console.log("v", v)
          return v
        } else {
          return fmt(v, defaultTokens)
        }
      })
      return Autolinker.link(this.messageStyling ? joinStr.join(" ") : escaped, {
        stripTrailingSlash: false,
        className: 'chatLink',
        truncate: { length: 50, location: 'smart' },
        replaceFn : function( match ) {
          var tag = match.buildTag();         // returns an `Autolinker.HtmlTag` instance for an <a> tag
          tag.setInnerHtml(match.matchedText);  // sets the inner html for the anchor tag
          return tag;
        }
      })
    }
  }
}
</script>

<style scoped>
a.chatLink {
  color: inherit !important;
}
</style>
