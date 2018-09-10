<template lang="pug">
#app
  UserList(
    :userList="$data.userList"
  )
  ChatList(
    :chatList="$data.chatList",
    @deleteChat="onDeleteChat"
  )
  ChatForm(
    :userName="userName",
    @changeName="onChangeName",
    @submitChat="onSubmitChat"
  )
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import UserList from '@/components/UserList.vue';
import ChatList from '@/components/ChatList.vue';
import ChatForm from '@/components/ChatForm.vue';

import firebase from 'firebase';
import 'firebase/database';

const app = firebase.initializeApp({
  apiKey: 'AIzaSyAuxlZjh1j6vnRztVllgkIG-yzpBJTGTDI',
  authDomain: 'testfirebase-21274.firebaseapp.com',
  databaseURL: 'https://testfirebase-21274.firebaseio.com',
  projectId: 'testfirebase-21274',
  storageBucket: 'testfirebase-21274.appspot.com',
  messagingSenderId: '220668028836'
});

const db = firebase.database();
const chatRef = db.ref('/test/chat/all');
const usersRef = db.ref('/test/chat/users');
const userRef = usersRef.push();
userRef.set({
  name: ''
});
userRef.onDisconnect().remove();

@Component({
  components: {
    UserList,
    ChatList,
    ChatForm,
  },
})
export default class App extends Vue {
  userName = '';
  chatList = {};
  userList = {};

  created() {
    chatRef.on('value', (snapshot: any) => {
      this.$data.chatList = snapshot.val() || {};
    });
    usersRef.on('value', (snapshot: any) => {
      this.$data.userList = snapshot.val() || {};
    });
  }

  /**
   * チャットの削除
   * @param key - chatのキー
   */
  onDeleteChat(key: string) {
    chatRef.child(key).remove();
  }

  onChangeName(name: string) {
    userRef.set({
      name
    });
    this.$data.userName = name;
  }

  /**
   * Chat送信のコールバック
   * @param name - 名前
   * @param text - テキスト
   */
  onSubmitChat(name: string, message: string) {
    // チャットを追加
    chatRef.push({
      name,
      message,
      created: firebase.database.ServerValue.TIMESTAMP
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
