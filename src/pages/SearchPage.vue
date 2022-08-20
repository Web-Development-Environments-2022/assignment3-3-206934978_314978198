<template>
  <div class="container">
    <h1 class="title">Search Page</h1>
    <b-navbar toggleable="lg" type="light" variant="light">
      <b-navbar-nav>
        <b-nav-form @submit.prevent="onSearch" @reset.prevent="onReset">
          <b-form-input
            variant="outline-secondary"
            class="mr-sm-2"
            placeholder="Search"
            type="text"
            v-model="query"
            @change="onChangeQuery"
          ></b-form-input>
        </b-nav-form>
        <!--  @keyup.enter="onChangeQuery" -->
        <b-nav-item-dropdown
          text="Number of search results"
          @submit.prevent="onSearch"
          @reset.prevent="onReset"
        >
          <b-dropdown-item
            v-for="(num, key) in numbers"
            :key="key"
            @click="number = num"
          >
            {{ num }}
          </b-dropdown-item>
        </b-nav-item-dropdown>

        <b-nav-item-dropdown
          text="Cuisine"
          @submit.prevent="onSearch"
          @reset.prevent="onReset"
        >
          <b-dropdown-item
            v-for="(cus, key) in cuisines"
            :key="key"
            @click="form.cuisine = cus"
          >
            {{ cus }}
          </b-dropdown-item>
        </b-nav-item-dropdown>

        <b-nav-item-dropdown
          text="Diet"
          @submit.prevent="onSearch"
          @reset.prevent="onReset"
        >
          <b-dropdown-item
            v-for="(d, key) in diets"
            :key="key"
            @click="form.diet = d"
          >
            {{ d }}
          </b-dropdown-item>
        </b-nav-item-dropdown>

        <b-nav-item-dropdown
          text="Intolerances"
          @submit.prevent="onSearch"
          @reset.prevent="onReset"
        >
          <b-dropdown-item
            v-for="(into, key) in intoleranceses"
            :key="key"
            @click="form.intolerances = into"
          >
            {{ into }}
          </b-dropdown-item>
        </b-nav-item-dropdown>

        <b-nav-item-dropdown
          text="Sort by"
          @submit.prevent="onSearch"
          @reset.prevent="onReset"
        >
          <b-dropdown-item
            href="#"
            :variant="this.sort_time"
            @click="sortBy('time')"
            >Recipe's preparation time</b-dropdown-item
          >
          <b-dropdown-item
            href="#"
            :variant="this.sort_popularity"
            @click="sortBy('popularity')"
            >Recipe's popularity</b-dropdown-item
          >
        </b-nav-item-dropdown>
        <b-nav-form @submit.prevent="onSearch" @reset.prevent="onReset">
          <b-button
            variant="outline-secondary"
            class="my-2 my-sm-0"
            type="submit"
            @click="onSearch"
          >
            <b-icon icon="search" aria-hidden="true"></b-icon>
          </b-button>
          <b-button
            variant="outline-secondary"
            class="my-2 my-sm-0"
            type="reset"
            @click="onReset"
            >Reset
          </b-button>
        </b-nav-form>
      </b-navbar-nav>
    </b-navbar>
    <div v-if="searched > 0">
      <RecipePreviewList
        title="Search Results"
        :log_in="Boolean($root.store.username)"
        :searched="searched"
        :query="query"
        :number="number"
        :cuisine="form.cuisine"
        :diet="form.diet"
        :intolerance="form.intolerances"
        :sort="sortby"
      />
    </div>
    <div v-else-if="$root.store.lastSearch">
      <RecipePreviewList
        title="Last Search"
        :log_in="Boolean($root.store.username)"
      />
    </div>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
import diets from "../assets/diet";
import numbers from "../assets/numbers";
import cuisines from "../assets/cuisines";
import intoleranceses from "../assets/intoleranceses";
export default {
  name: "Search",
  components: {
    RecipePreviewList,
  },

  data() {
    return {
      searched: 0,
      query: "",
      number: "",
      form: {
        cuisine: "",
        diet: "",
        intolerances: "",
      },
      submitError: [],
      // results: [] || this.$root.store.lastSearch,
      numbers: [{ value: null, text: "", disabled: true }],
      cuisines: [{ value: null, text: "", disabled: true }],
      diets: [{ value: null, text: "", disabled: true }],
      intoleranceses: [{ value: null, text: "", disabled: true }],
      sortby: "",
      lastsearch: "",
      sort_time: "s",
      sort_popularity: "s",
    };
  },

  async created() {
    console.log("in Search Page");
    this.getLastSearch();
    // if (sessionStorage.getItem("lastRes") != undefined) {
    //   this.results = sessionStorage.getItem("lastRes");
    // }
  },

  mounted() {
    this.numbers.push(...numbers);
    this.numbers.shift();
    this.number = this.numbers[0];

    this.cuisines.push(...cuisines);
    this.cuisines.shift();

    this.diets.push(...diets);
    this.diets.shift();

    this.intoleranceses.push(...intoleranceses);
    this.intoleranceses.shift();

    // if (sessionStorage.getItem("lastRes") != undefined) {
    //   this.results = sessionStorage.getItem("lastRes");
    // }
  },

  methods: {
    onChangeQuery() {
      console.log("in onChangeQuery");
      this.$emit("query", this.query);
      // this.$emit("searched", this.searched);
    },

    onSearch() {
      console.log("in OnSearch");
      this.searched += 1;
      // this.$emit("searched", this.searched);
      this.lastsearch = this.query;
    },

    onReset() {
      this.query = "";
      this.number = this.numbers[0];
      this.form.diet = "";
      this.form.cuisine = "";
      this.form.intolerances = "";
      this.sortby = "";
      this.sort_time = "s";
      this.sort_popularity = "s";
      this.searched = 0;
      // this.$nextTick(() => {
      //   this.$v.$reset();
      // });
    },

    sortBy(sort_type) {
      if (sort_type == "time") {
        if (this.sortby == "readyInMinutes") {
          this.sortby = "";
          this.sort_time = "s";
        } else {
          this.sortby = "readyInMinutes";
          this.sort_time = "p";
          this.sort_popularity = "s";
        }
      } else if (sort_type == "popularity") {
        if (this.sortby == "aggregateLikes") {
          this.sortby = "";
          this.sort_popularity = "s";
        } else {
          this.sortby = "aggregateLikes";
          this.sort_popularity = "p";
          this.sort_time = "s";
        }
      }

      if (searched > 0) {
        this.searched += 1;
      }
    },

    async getLastSearch() {
    //   let response;
    //   try {
    //     console.log( this.$root.store.server_domain + "/user/lastSearch");
    //     response = await this.axios.get(
    //       this.$root.store.server_domain + "/user/lastSearch",
    //       { withCredentials: true }
    //     );
    //     console.log(response);
    //     this.lastsearch = response.data;
    //   } catch (error) {
    //     console.log(error);
    //   }
    },

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
            this.form.cuisine == undefined ? "" : this.form.cuisine +
            "&diet=" +
            this.form.diet == undefined ? "" : this.form.diet +
            "&intolerances=" +
            this.form.intolerances == undefined ? "" : this.form.intolerances
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
          // sessionStorage.setItem(
          //     "lastRes",
          //     JSON.stringify(this.results)

          // );
          
        }
      } catch (err) {
        console.log(err.response);
        this.submitError = err.response.data.message;
      }
    },
  },
};
</script>

