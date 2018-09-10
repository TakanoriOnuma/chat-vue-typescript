<template lang="pug">
#app
  ChatList(
    :chatList="$data.chatList"
  )
  ChatForm(
    @submitChat="onSubmitChat"
  )
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import ChatList from '@/components/ChatList.vue';
import ChatForm from '@/components/ChatForm.vue';

@Component({
  components: {
    ChatList,
    ChatForm,
  },
})
export default class App extends Vue {
  chatList = [{ name: 'name', message: 'テスト' }];

  /**
   * Chat送信のコールバック
   * @param name - 名前
   * @param text - テキスト
   */
  onSubmitChat(name: string, message: string) {
    this.$data.chatList.push({
      name,
      message
    });
  }
}
</script>

<style lang="scss">
ul, li {
  margin: 0;
  padding: 0;
}
</style>
