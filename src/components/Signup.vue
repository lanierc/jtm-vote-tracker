<template>
  <div>
    <h2>Signup</h2>
    <p class="error" v-if="error !== ''"><span>Error:</span> {{ error }}</p>
    <form @submit.prevent="doSignup">
      <div>
        <input type="text" v-model="email" placeholder="Email Address" />
      </div>
      <div>
        <input type="password" v-model="password" placeholder="Password" />
      </div>
      <div>
        <input
          type="password"
          v-model="verifyPassword"
          placeholder="Verify Password"
        />
      </div>
      <div>
        <input type="text" v-model="username" password="Username" />
      </div>
      <button type="submit">Signup</button>
      <router-link to="/">Login</router-link>
    </form>
  </div>
</template>

<script>
import firebase, { auth } from "../constants/firebase.js";
export default {
  name: "Signup",
  data() {
    return {
      email: "",
      password: "",
      verifyPassword: "",
      username: "",
      error: "",
    };
  },
  methods: {
    doSignup: function () {
      const { email, password, verifyPassword, username } = this.$data;
      if (password === verifyPassword) {
        auth.createUserWithEmailAndPassword(email, password).catch((err) => {
          console.error(err.code, err.message);
          this.error = err.message;
        });
        auth.onAuthStateChanged((user) => {
          const dbRef = firebase.database().ref(`users/${user.uid}`);
          dbRef.set({ username, email, isAdmin: false });
          window.location.assign("/");
        });
      } else {
        console.error("Passwords don't match.");
        this.error = "Passwords don't match.";
      }
    },
  },
};
</script>
