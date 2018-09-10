<template lang="pug">
div
  form(@submit="onSubmit")
    input(type="text", placeholder="名前", :value="$props.userName", @input="onInput")
    br
    input(type="text", placeholder="メッセージ", v-model="$data.message")
    br
    button(type="submit", :disabled="!_canSubmittion") 送信
</template>

<script lang="ts">
import { Component, Prop, Emit, Vue } from 'vue-property-decorator';

@Component
export default class ChatForm extends Vue {
  @Prop() private userName: string = '';
  private message: string = '';

  @Emit('submitChat')
  submitSendChat(name: string, message: string) {}

  @Emit('changeName')
  emitChangeName(name: string) {}

  get _canSubmittion() {
    const { name, message } = this.$data;
    return name !== '' && message !== '';
  }

  onInput(e: any) {
    this.emitChangeName(e.target.value);
  }

  onSubmit(e: any) {
    e.preventDefault();
    this.submitSendChat(this.$props.userName, this.$data.message);
    this.$data.message = '';
  }
}
</script>

