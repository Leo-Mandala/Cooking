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

<style scoped>
button {
  border-radius: 8px;
  background-color: #009688; /* Marmiton's green color */
  border: none;
  text-decoration: none;
  display: inline-block;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.3);
  font-size: 15px;
  padding: 5px 10px;
  margin-right: 10px;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s ease;
}
</style>