<template>
  <div class="container">
    <h1 class="title">Search Page</h1>
    <b-form class="form-inline" @submit.prevent="onSearch">
      <b-form-group class="input-group mb-3">
        <b-form-input
          id="input-group-search"
          type="text"
          v-model="query"
          class="form-control"
          placeholder="Find a Recipe"
          aria-label="Find a Recipe"
          aria-describedby="button-addon2"
        />
        <b-form-invalid-feedback>
          Recipe's name is required
        </b-form-invalid-feedback>
        <div class="input-group-append">
          <b-button size="" class="mb-3" type="submit">
            <b-icon icon="search" aria-hidden="true"></b-icon>
          </b-button>
        </div>
      </b-form-group>
    </b-form>
    <RecipePreviewList title="Search Recipe"></RecipePreviewList>
  </div>
</template>

<script>
import { BIconHandThumbsDown } from "bootstrap-vue";
import RecipePreviewList from "../components/RecipePreviewList.vue";
export default {
  name: "Search",
  components: {
    RecipePreviewList,
  },

  data() {
    return {
      query: "",
      number: 5,
      cuisine: "",
      diet: "",
      intolerances: "",
      submitError: undefined,
    };
  },

  methods: {
    async Search() {
      try {
        console.log(this.query);
        console.log(this.number);
        const response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          this.$root.store.server_domain + "/recipes/search?searchQuery=" + this.query + "&number=" + this.number + "&cuisine=" + this.cuisine + "&diet=" + this.diet + "&intolerances=" + this.intolerances        
        );

        console.log(response);

        const recipes_id = response.data;
        const recipes = [];

        for (let i = 0; i < this.number; i++) {
          console.log(recipes_id[i]);
          recipes[i] = await this.axios.get(
            // "https://test-for-3-2.herokuapp.com/user/Register",
            this.$root.store.server_domain + "/recipes/:recipeId?id=" + recipes_id[i]
          );
        }

        console.log(recipes);
      } catch (err) {
        console.log(err.response);
        this.submitError = err.response.data.message;
      }
    },

    onSearch() {
      this.Search();
    },
  },
};
</script>

