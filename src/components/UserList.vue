<template lang="pug">
div
  p user list
  ul.list
    template(v-for="key in Object.keys($props.userList)")
      li.item(:key="key") {{ $props.userList[key].name || 'ユーザ' }}
</template>

<script lang="ts">
import { Component, Prop, Emit, Vue } from 'vue-property-decorator';

@Component
export default class UserList extends Vue {
  @Prop() userList!: any;
}
</script>

<style lang="scss" scoped>
.item {
  display: inline-block;

  & + & {
    &::before {
      display: inline-block;
      content: '、';
    }
  }
}
</style>

