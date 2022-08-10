<template>
  <div id="navbar">
    <b-navbar class="navbar navbar-light" style="background-color: lightblue">
      <b-navbar-nav>
        <b-nav-item href="#">
          <router-link :to="{ name: 'main' }">Main</router-link>
        </b-nav-item>
        <b-nav-item>
          <router-link :to="{ name: 'search' }">Search</router-link>
        </b-nav-item>
        <b-nav-item> About </b-nav-item>
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
          <b-dropdown-item></b-dropdown-item>
        </b-nav-item-dropdown>

        <b-navbar-item>
            Hello {{ $root.store.username }}
        </b-navbar-item>
        <b-navbar-item @click="Logout">
            Logout
        </b-navbar-item>
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
  </div>
</template>

<script>
import NewRecipeModal from "./NewRecipeModal.vue";

export default {
  name: "NavBarApp",
  comments: { NewRecipeModal },
  props: {
    logout: { type: Function },
  },
  methods: {
    Logout() {
      this.logout();
    },
  },
  components: { NewRecipeModal },
};
</script>

<style scoped>
</style>
