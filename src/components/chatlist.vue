<template>
  <div class="messages" v-chat-scroll="{always: false}">
    <div v-for="line in chat" :style="styleLine(line)">
      <span :title="$moment(line.time, 'MMMM Do, Y')">
        &lt;{{$moment(line.time, "HH:mm:ss")}}&gt;
      </span>
      {{prefix(line)}}{{line.name}}[{{line.level}}]:
      <span class="message" v-html="render(line.message, line.admin||line.mod)"></span>
    </div>
  </div>
</template>

<script>
import render from '../render'

export default {
  name: 'chatlist',
  props: ['chat'],
  methods: {
    render,
    prefix: (line) => {
      let r = ''
      if (line.hardcore) r += '[H]'
      if (line.admin) r += '<ADMIN>'
      if (line.mod) r += '<MOD>'
      return r
    },
    styleLine: (line) => {
      return {
        color: line.level >= 100 ? 'black' : line.color,
        textShadow: line.level >= 100 ? line.color + ' 0px 0px 2px, ' + line.color + ' 0px 0px 6px' : '',
        fontWeight: line.level >= 100 ? 'bold' : ''
      }
    }
  }
}
</script>

<style scoped>
.messages {
  flex: auto;
  overflow-y: scroll;
  word-break: break-word;
}
</style>

<style>
.message img {
  vertical-align: top;
  max-height: 200px;
}

.messages ::selection {
  background: #808080;
}
</style>
