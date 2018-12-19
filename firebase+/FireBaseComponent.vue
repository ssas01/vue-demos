<template>
    <div id="app">
        <ul is="transition-group">
            <li v-for="user in users" class="user" :key="user['.key']">
              <span>{{user.name}} - {{user.email}}</span>
              <button v-on:click="removeUser(user)">X</button>
            </li>
        </ul>
        <form id="form" v-on:submit.prevent="addUser">
          <input type="text" v-model="newUser.name" placeholder="Username">
          <input type="email" v-model="newUser.email" placeholder="email@email.com">
          <input type="submit" value="Add User">
        </form>
        <ul class="errors">
          <li v-show="!validation.name">Name cannot be empty.</li>
          <li v-show="!validation.email">Please provide a valid email address.</li>
        </ul>
    </div>
</template>

<script>
// 正则和连接 firebase 取出数据
var emailRE = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/

// Setup Firebase
var config = {
  apiKey: "AIzaSyAi_yuJciPXLFr_PYPeU3eTvtXf8jbJ8zw",
  authDomain: "vue-demo-537e6.firebaseapp.com",
  databaseURL: "https://vue-demo-537e6.firebaseio.com"
}
firebase.initializeApp(config)

var usersRef = firebase.database().ref('users')

var uRegexp = /./;
var eRegexp = /./;
export default {
  // created ? mounted ?
  mounted() {
    // 请求数据
    this.users = ["from firebase."];
  },
  data() {
    return {
        newUser: {
            name: '',
            email: ''
        }
    }
  },
  firebase: {
    users: usersRef
  },
  computed: {
    // 表单验证，放到计算属性中
    validation() {
      return {
        name: !!this.newUser.name.trim(),
        email: emailRE.test(this.newUser.email)
      };
    },
    isValidate() {
      var validation = this.validation;
      return Object.keys(validation).every(function(key) {
        return validation[key];
      });
    }
  },
  methods: {
    addUser() {
      if (this.isValidate) {
        usersRef.push(this.newUser)
        this.username = "";
        this.email = "";
      }
    },
    removeUser: function (user) {
      usersRef.child(user['.key']).remove()
    }
  }
};

/*
    表单验证 --- 计算属性，非空验证
    submit prevent / input type=submit
*/
</script>

<style>
.errors {
  color: red;
}
</style>
