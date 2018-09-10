<template lang="pug">
div
  p.title chat list
  template(v-if="Object.keys($props.chatList).length <= 0")
    .list
      div 読み込み中
  template(v-else)
    transition-group(
      tag="ul",
      class="list",
      name="flip-list",
      appear,
      @before-appear="onBeforeAppear",
      @after-appear="onAfterAppear"
    )
      template(v-for="(key, index) in Object.keys($props.chatList)")
        li.chat(:key="key", :data-index="index")
          .chat__inner
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

  onBeforeAppear(el: any) {
    // 要素に直接手を加える
    el.style.transitionDelay = `${el.dataset.index * 0.1}s`;
  }

  onAfterAppear(el: any) {
    el.style.transitionDelay = '';
  }
}
</script>

<style lang="scss" scoped>
.title {
  border-bottom: solid 2px #00c;
}

.list {
  position: relative;
  height: calc(100vh - 300px);
  border: solid 1px #ccc;
  overflow-y: scroll;
}

// transition-groupアニメーション
.flip-list {
  // 要素が動くときにtransitionを設定する（.chatでtransitionを設定しているため-moveで書く必要はない）
  // &-move {
  //   transition: transform 0.5s;
  // }

  // 要素が入るときのアニメーション
  &-enter {
    &-active {
      opacity: 0;
      transform: translate3d(0, -20px, 0);
    }
    &-to {
      opacity: 1;
      transform: translate3d(0, 0, 0);
    }
  }

  // 要素が消える時のアニメーション
  &-leave {
    &-active {
      position: absolute;
    }
    &-to {
      opacity: 0;
      transform: translate3d(0, -20px, 0);
    }
  }
}

.chat {
  width: 100%;
  border: solid 1px #ccc;
  background: #fff;
  transition: all 0.5s;

  &__inner {
    position: relative;
  }

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
