<template lang="pug">
div
  p chat list
  ul.list
    template(v-for="key in Object.keys(chatList)")
      li.chat(:key="key")
        div {{ chatList[key].name }}
        div {{ chatList[key].message }}
        .chat__delete(
          @click="emitDeleteChat(key)"
        )
</template>

<script lang="ts">
import { Component, Prop, Emit, Vue } from 'vue-property-decorator';

@Component
export default class ChatForm extends Vue {
  @Prop() chatList!: any;

  @Emit('deleteChat')
  emitDeleteChat(key: string) {}
}
</script>

<style lang="scss" scoped>
.list {
  height: calc(100vh - 200px);
  border: solid 1px #ccc;
  overflow-y: scroll;
}

.chat {
  position: relative;
  border: solid 1px #ccc;

  &__delete {
    position: absolute;
    top: 0;
    right: 0;
    width: 30px;
    height: 30px;

    &::before, &::after {
      position: absolute;
      top: 50%;
      left: 50%;
      width: 80%;
      height: 2px;
      background: #ccc;
      content: '';
    }

    &::before {
      transform: translate3d(-50%, -50%, 0) rotate(45deg);
    }

    &::after {
      transform: translate3d(-50%, -50%, 0) rotate(135deg);
    }
  }
}
</style>
