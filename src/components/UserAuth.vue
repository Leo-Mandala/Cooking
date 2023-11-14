<template>
  <div v-if="user">
    <h3>Bienvenue, {{ user.username }} let's cook</h3>
  </div>
  <div v-else>
    <form @submit.prevent="login">
      <h4>connectez vous pour ajouter ou modifier une recette</h4>
      <input type="text" v-model="username" placeholder="Entrer votre nom d'utilisateur" />
      <button type="submit">Connectez vous</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'UserAuth',
  data() {
    return {
      username: '',
      user: null,
    };
  },
  mounted() {
    this.user = JSON.parse(localStorage.getItem('user'));
    this.$emit('user-authenticated', this.user !== null);
  },
  methods: {
    login() {
      if (this.username) {
        this.user = { username: this.username };
        localStorage.setItem('user', JSON.stringify(this.user));
        this.$emit('user-authenticated', true);
      }
    },
  },
};
</script>
