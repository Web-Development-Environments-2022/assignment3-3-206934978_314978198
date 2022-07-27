<template>
  <div class="container">
    <h1 class="title">Search Page</h1>
    <b-form class="form-inline" @submit.prevent="onSearch" @reset.prevent="onReset">
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
        <div class="input-group-append">
          <b-button size="" class="mb-3" type="submit">
            <b-icon icon="search" aria-hidden="true"></b-icon>
          </b-button>
        </div>
      </b-form-group>
      <b-form-group
        id="input-group-number"
        label-cols-sm="3"
        label="Number:"
        label-for="number"
      >
        <b-form-select
          id="number"
          v-model="number"
          :options="numbers"
        ></b-form-select>
      </b-form-group>
      <b-form-group
        id="input-group-cuisine"
        label-cols-sm="3"
        label="Cuisine:"
        label-for="cuisine"
      >
        <b-form-select
          id="cuisine"
          v-model="cuisine"
          :options="cuisines"
        ></b-form-select>
      </b-form-group>
      <b-form-group
        id="input-group-diet"
        label-cols-sm="3"
        label="Diet:"
        label-for="diet"
      >
        <b-form-select
          id="diet"
          v-model="diet"
          :options="diets"
        ></b-form-select>
      </b-form-group>
      <b-form-group
        id="input-group-intolerances"
        label-cols-sm="3"
        label="Intolerances:"
        label-for="intolerances"
      >
        <b-form-select
          id="intolerances"
          v-model="intolerances"
          :options="intoleranceses"
        ></b-form-select>
      </b-form-group>
      <b-button type="reset">Reset</b-button>
    </b-form>
    <b-container v-if="searched > 0">
      <b-row>
        <b-col v-for="r in this.results" :key="r.id">
          <RecipePreview class="recipePreview" :recipe="r" />
        </b-col>
      </b-row>
    </b-container>
    <!-- <RecipePreview :recipe="results[0].data"></RecipePreview> -->
    <!-- <RecipePreviewList title="Search"></RecipePreviewList> -->
  </div>
</template>

<script>
import RecipePreview from "../components/RecipePreview.vue";
import diets from "../assets/diet";
import numbers from "../assets/numbers";
import cuisines from "../assets/cuisines";
import intoleranceses from "../assets/intoleranceses";
export default {
  name: "Search",
  components: {
    RecipePreview,
  },

  data() {
    return {
      searched: 0,
      query: "",
      number: 5,
      cuisine: "",
      diet: "",
      intolerances: "",
      submitError: undefined,
      results: [] || this.$root.store.lastSearch,
      numbers: [{ value: null, text: "", disabled: true }],
      cuisines: [{ value: null, text: "", disabled: true }],
      diets: [{ value: null, text: "", disabled: true }],
      intoleranceses: [{ value: null, text: "", disabled: true }],
    };
  },

  mounted() {
    this.numbers.push(...numbers);
    this.cuisines.push(...cuisines);
    this.diets.push(...diets);
    this.intoleranceses.push(...intoleranceses);
  },

  methods: {
    async Search() {
      try {
        const response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          this.$root.store.server_domain +
            "/recipes/search?searchQuery=" +
            this.query +
            "&number=" +
            this.number +
            "&cuisine=" +
            this.cuisine +
            "&diet=" +
            this.diet +
            "&intolerances=" +
            this.intolerances
        );

        if (response.data === "There is no results!") {
          this.$root.toast(
            "Search a Recipe",
            "There is no results for this search",
            "failure"
          );
        } else {
          const recipes_id = response.data;
          const recipes = [];

          for (let i = 0; i < recipes_id.length; i++) {
            recipes[i] = await this.axios.get(
              // "https://test-for-3-2.herokuapp.com/user/Register",
              this.$root.store.server_domain +
                "/recipes/:recipeId?id=" +
                recipes_id[i]
            );
          }

          for (let i = 0; i < recipes.length; i++) {
            this.results[i] = recipes[i].data;
          }
          console.log(this.results);

          console.log(this.searched);

          this.$root.store.lastSearch = this.results;

        }
      } catch (err) {
        console.log(err.response);
        this.submitError = err.response.data.message;
      }
    },

    onSearch() {
      this.searched += 1;
      this.Search();
    },

    onReset() {
      this.query = "";
      this.number = 5;
      this.diet = "";
      this.cuisine = "";
      this.intolerances = "";
      // this.$nextTick(() => {
      //   this.$v.$reset();
      // });
    }
  },
};
</script>

