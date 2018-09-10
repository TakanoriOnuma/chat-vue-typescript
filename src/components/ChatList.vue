<template lang="pug">
div
  p chat list
  ul.list
    template(v-for="chat in chatList")
      li.chat
        div {{ chat.name }}
        div {{ chat.message }}
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class ChatForm extends Vue {
  @Prop() chatList!: any[];
}
</script>

<style lang="scss" scoped>
.list {
  height: calc(100vh - 200px);
  border: solid 1px #ccc;
  overflow-y: scroll;
}

.chat {
  border: solid 1px #ccc;
}
</style>
