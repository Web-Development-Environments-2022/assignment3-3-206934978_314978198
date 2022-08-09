<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <div v-if="no_results">
      <h4>There is no results ☹️</h4>
    </div>
    <div v-else>
      <b-row>
        <b-row v-for="r in recipes" :key="r.id">
          <RecipePreview class="recipePreview" :recipe="r" :log_in="log_in" :title="title" />
        </b-row>
      </b-row>
    </div>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview,
  },
  props: {
    title: {
      type: String,
      required: true,
    },
    log_in: {
      type: Boolean,
      required: false,
      default: false,
    },
  },
  data() {
    return {
      recipes: [],
      no_results: false,
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        if (this.title == "Explore This Recipes") {
          await this.randomRecipes();
        } else if (this.title == "Last Viewed Recipes") {
          await this.lastViewedRecipes();
        } else if (this.title == "My Favorite Recipes") {
          await this.favoriteRecipes();
        } else if (this.title == "My Recipes") {
          await this.myRecipes();
        }
      } catch (error) {
        console.log(error);
      }
    },

    async randomRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/random"
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );

        // console.log(response);
        const recipes = response.data.recipes;
        this.recipes = [];
        this.recipes.push(...recipes);
      } catch (error) {
        console.log(error);
      }
    },

    async lastViewedRecipes() {
      try {
        let response;
        let returned_recipes = [];

        console.log(this.$root.store.server_domain + "/recipes/watched");

        response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/watched",
          { withCredentials: true }
        );

        console.log(response);

        if (response == []) {
          this.no_results = true;
        } else {
          returned_recipes = response.data;
          this.recipes = [];
          this.recipes.push(...returned_recipes);
        }
      } catch (error) {
        console.log(error);
      }
    },

    async favoriteRecipes() {
      try {
        let response;
        let returned_recipes;

        console.log(this.$root.store.server_domain + "/user/favorites");

        response = await this.axios.get(
          this.$root.store.server_domain + "/user/favorites",
          { withCredentials: true }
        );

        console.log(response);

        if (response == []) {
          this.no_results = true;
        } else {
          returned_recipes = response.data;
          this.recipes = [];
          this.recipes.push(...returned_recipes);
        }
      } catch (error) {
        console.log(error);
      }
    },

    async myRecipes() {
      try {
        let response;
        let returned_recipes;

        response = await this.axios.get(
          this.$root.store.server_domain + "/user/myRecipes",
          { withCredentials: true }
        );

        console.log(response);

        if (response.data.length == 0) {
          this.no_results = true;
        } else {
          returned_recipes = response.data;
          this.recipes = [];
          this.recipes.push(...returned_recipes);
        }
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
