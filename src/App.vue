<template>
  <div id="app">
    <div id="nav">
      <router-link :to="{ name: 'main' }">Vue Recipes</router-link>|
      <router-link :to="{ name: 'search' }">Search</router-link>|
      <span v-if="!$root.store.username">
        Guest:
        <router-link :to="{ name: 'register' }">Register</router-link>|
        <router-link :to="{ name: 'login' }">Login</router-link>|
      </span>
      <span v-else>
        <router-link :to="{ name: 'myrecipes' }">My Recipes</router-link>|
        <router-link :to="{ name: 'myfavoriterecipes' }"
          >My Favorite Recipes</router-link
        >
        {{ $root.store.username }}: <button @click="Logout">Logout</button>|
        <!-- <b-button v-b-modal.modal-prevent-closing>Add New Recipe</b-button> -->
        <!-- <b-button v-b-modal.modal-prevent-closing>Add
        </b-button> -->
      </span>

      
      <div>
        <b-button v-b-modal.add-recipe-modal>Launch demo modal</b-button>
        <NewRecipeModal></NewRecipeModal>
        <!-- <b-modal id="modal-1" title="BootstrapVue">
          <p class="my-4">Hello from modal!</p>
        </b-modal> -->
      </div>
    </div>
    <router-view />
  </div>
</template>

<script>
import NewRecipeModal from "./components/NewRecipeModal";

export default {
  name: "App",
  components: { NewRecipeModal },
  methods: {
    async Logout() {
      try {
        const response = await this.axios.post(
          this.$root.store.server_domain + "/Logout"
        );
        this.$root.store.logout();
        if (response.message == "logout succeeded") {
          this.$root.toast("Logout", "User logged out successfully", "success");
          this.$router.push("/").catch(() => {
            this.$forceUpdate();
          });
        }
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
  },
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
