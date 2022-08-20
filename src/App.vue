
<template>
  <div id="app">
    <!-- <NavigationBar :logout="Logout"></NavigationBar> -->

    <!-- https://bootstrap-vue.org/docs/components/navbar#color-schemes -->

    <b-navbar>
      <b-navbar-nav>
        <b-nav-item href="#">
          <router-link :to="{ name: 'main' }">Main</router-link>
        </b-nav-item>
        <b-nav-item>
          <router-link :to="{ name: 'search' }">Search</router-link>
        </b-nav-item>
         <b-nav-item href="#">
          <router-link :to="{ name: 'about' }">
          About
          </router-link>
        </b-nav-item>
      </b-navbar-nav>

      <b-navbar-nav v-if="$root.store.username" id="loggedInUser">
        <b-nav-item v-b-modal.add-recipe-modal> Create New Recipe </b-nav-item>
        <NewRecipeModal />
        <b-nav-item-dropdown type="dark" variant="light" text="Personal">
          <b-dropdown-item href="#" id="favorites">
            <router-link :to="{ name: 'myfavoriterecipes' }">
              My Favorite Recipes
            </router-link>
          </b-dropdown-item>
          <b-dropdown-item id="myRecipes">
            <router-link :to="{ name: 'myrecipes' }">My Recipes</router-link>
          </b-dropdown-item>
          <b-dropdown-item>
            <router-link :to="{ name: 'myfamilyrecipes' }">My Family Recipes</router-link>
          </b-dropdown-item>
        </b-nav-item-dropdown>

        <b-nav-item>
            Hello {{ $root.store.username }}
        </b-nav-item>
        <b-nav-item @click="Logout">
            Logout
        </b-nav-item>
      </b-navbar-nav>

      <b-navbar-nav v-else>
        <b-nav-item>
            Hello Guest            
        </b-nav-item>
        <b-nav-item>
            <router-link :to="{ name: 'register' }">Register</router-link>
        </b-nav-item>
        <b-nav-item>
            <router-link :to="{ name: 'login' }">Login</router-link>
        </b-nav-item>
      </b-navbar-nav>
    </b-navbar>
    <router-view />
  </div>
</template>

<script>
import NewRecipeModal from "./components/NewRecipeModal";
// import NavigationBar from "./components/NavigationBar.vue";

export default {
  name: "App",
  components: { NewRecipeModal },
  //NavigationBar,
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
  font-family: Cursive,  Fantasy;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: DarkSeaGreen;
  color: #2c3e50;
  min-height: 100vh;
  background-image: url("/assets/appBackground.png");
  background: #FFE7E0;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

.navbar a {
  font-weight: bold;
  font-size: 22px;
  background-color: DarkSeaGreen;
}

.navbar a :hover {
  background: #dddddd;
  color: #333333;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

