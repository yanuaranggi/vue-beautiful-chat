<template>
  <div class="sc-message--text" :style="messageColors" v-loading="data.proccessUpload ? data.proccessUpload : false">
    <p v-html="messageText"></p>
    <p v-if="data.meta" class='sc-message--meta' :style="{color: messageColors.color}">{{data.meta}}</p>
  </div>
</template>

<script>
import escapeGoat from 'escape-goat'
import Autolinker from 'autolinker'
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
      const escaped = escapeGoat.escape(this.data.text)
      return Autolinker.link(this.messageStyling ? fmt(escaped, defaultTokens) : escaped, {
        stripTrailingSlash: false,
        className: 'chatLink',
        truncate: { length: 50, location: 'smart' },
        replaceFn : function( match ) {
          var tag = match.buildTag();         // returns an `Autolinker.HtmlTag` instance for an <a> tag
          tag.setInnerHtml(match.getAnchorHref());  // sets the inner html for the anchor tag

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
