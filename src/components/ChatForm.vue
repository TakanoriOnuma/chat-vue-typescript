<template lang="pug">
div
  form(@submit="onSubmit")
    input(type="text", placeholder="名前", v-model="$data.name")
    br
    input(type="text", placeholder="メッセージ", v-model="$data.message")
    br
    button(type="submit", :disabled="!_canSubmittion") 送信
</template>

<script lang="ts">
import { Component, Emit, Vue } from 'vue-property-decorator';

@Component
export default class ChatForm extends Vue {
  private name: string = '';
  private message: string = '';

  @Emit('submitChat')
  submitSendChat(name: string, message: string) {}

  get _canSubmittion() {
    const { name, message } = this.$data;
    return name !== '' && message !== '';
  }

  onSubmit(e: any) {
    e.preventDefault();
    this.submitSendChat(this.$data.name, this.$data.message);
    this.$data.message = '';
  }
}
</script>

