<template>
  <div>
    <div>
      <router-link
        :to="{
          name: 'recipe',
          params: { recipeId: recipe.id, user_recipe: user_recipe },
        }"
        class="recipe-preview"
      >
        <div>
          <b-card
            :img-src="recipe.image"
            img-alt="Image"
            img-top
            style="width: 22rem; height: 34rem; color: black"
          >
            <b-card-header
              :header="recipe.title"
              :border-variant="watchedColor"
              :header-border-variant="watchedColor"
              :header-bg-variant="watchedColor"
              style="width: 19.5rem; color: white"
            >
            </b-card-header>
            <b-card-body class="recipe-body">
              <b-avatar
                variant="transparent"
                icon="clock"
                size="2em"
              ></b-avatar>
              {{ recipe.readyInMinutes }} minutes |
              <b-avatar
                variant="transparent"
                icon="hand-thumbs-up"
                size="2em"
              ></b-avatar>
              {{ recipe.aggregateLikes }} likes
              <!-- <b-avatar v-if="watched" variant="tra"></b-avatar> -->
            </b-card-body>
            <b-card-body class="recipe-body">
              <b-avatar
                v-if="recipe.vegan"
                variant="tranparent"
                :src="require('@/assets/vegan.png')"
                style="width: 2.5em; height: 3.3em"
              >
              </b-avatar>
              <b-avatar
                v-if="recipe.vegetarian"
                variant="tranparent"
                :src="require('@/assets/vegetarian.png')"
                style="width: 4.5em; height: 3em"
              >
              </b-avatar>
              <b-avatar
                v-if="recipe.glutenFree"
                variant="tranparent"
                :src="require('@/assets/gluten_free.png')"
                style="width: 3.8em; height: 3em"
              >
              </b-avatar>
            </b-card-body>
            <b-card-footer v-if="log_in" class="recipe-footer">
              <!-- <b-card-body class="recipe-body"> -->
                <b-button
                  v-if="!this.favorite"
                  @click="addToFavorite"
                  variant="outline-dark"
                >
                  <b-icon-star></b-icon-star>
                </b-button>

                <b-button v-else variant="outline-success" disabled="false">
                  <b-icon-star></b-icon-star>
                </b-button>
              <!-- </b-card-body> -->
            </b-card-footer>
          </b-card>
        </div>
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    this.axios.get(this.recipe.image).then((i) => {
      this.image_load = true;
    });
  },

  data() {
    return {
      image_load: false,
      favorite: undefined,
      watchedColor: "dark",
    };
  },

  props: {
    recipe: {
      type: Object,
      required: true,
    },
    user_recipe: {
      type: Boolean,
      default: false,
    },
    log_in: {
      type: Boolean,
      required: false,
      default: false,
    },
  },
  methods: {
    async addToFavorite() {
      const recipe = { recipeId: this.$route.params.recipeId };
      let response;

      try {
        response = await this.axios.post(
          this.$root.store.server_domain + "/user/favorites",
          {
            rec_id: recipe.recipeId,
            withCredentials: true,
          }
        );
        this.favorite = true;
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
  },
  async created() {
    let watched_response = await this.axios.get(
      this.$root.store.server_domain +
        "/recipes/isWatched?recipeId=" +
        this.recipe.id,
      { withCredentials: true }
    );

    if (watched_response.data == true) {
      this.watchedColor = "info";
    }

    let favorite_response = await this.axios.get(
      this.$root.store.server_domain +
        "/user/isAFavorites?recipeId=" +
        this.recipe.id,
      { withCredentials: true }
    );

    if (favorite_response.data == true) {
      this.favorite = true;
    }
  },
};
</script>

<style scoped>
.recipe-preview {
  display: inline-block;
  /* width: 90%;
  height: 100%; */
  position: relative;
  margin: 10px 10px;
}

.recipe-preview .recipe-body {
  width: 20rem;
  height: 4rem;
  position: relative;
  padding: 5px;
}

/* .recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 98%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover; 
}*/

.recipe-preview .recipe-footer {
  background: white;
  height: 5rem;
  width: 20rem;
  overflow: hidden;
  border: 0ch;
  padding: 5px;
}

/* .recipe-preview .recipe-footer .recipe-body {
  width: 20rem;
  height: 3rem;
  position: relative;
  padding: 10px;
} */

/* .recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
} */

/* .recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
} */

/* .recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
} */
</style>
