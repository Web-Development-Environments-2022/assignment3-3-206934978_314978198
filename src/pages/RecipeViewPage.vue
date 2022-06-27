<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <img :src="recipe.imageUrl" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>Likes: {{ recipe.popularity }} likes</div>
              <div>Servings: {{ recipe.servings }} </div>
              <div v-if="recipe.vegan">Vegan: Yes </div>
              <div v-else-if="!recipe.vegan">Vegan: No </div>
              <div v-if="recipe.vegetarian">Vegetarian: Yes </div>
              <div v-else-if="!recipe.vegetarian">Vegetarian: No </div>
              <div v-if="recipe.gluten_free">Gluten Free: Yes </div>
              <div v-else-if="!recipe.gluten_free">Gluten Free: No </div>
            </div>
            Ingredients:
            <ul>
              <li
                v-for="(r, index) in recipe.ingredients"
                :key="index + '_' + r.id"
              >
                {{ r.original }}
              </li>
            </ul>
          </div>
          <div class="wrapped">
            Instructions:
            <ol>
              <li v-for="s in recipe.analyze_Instructions" :key="s.number">
                {{ s.step }}
              </li>
            </ol>
          </div>
        </div>
      </div>
      <!-- <pre>
      {{ $route.params }}
      {{ recipe }}
    </pre
      > -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recipe: null
    };
  },
  async created() {
    try {
      let response;
      // response = this.$route.params.response;

      try {
        response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/recipes/info",
          this.$root.store.server_domain + "/recipes/fullDetailes?recipeid=" + this.$route.params.recipeId);

        // console.log("response.status", response.status);
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }

      console.log(response.data);

      let {
        analyze_Instructions,
        instructions,
        ingredients,
        popularity,
        readyInMinutes,
        imageUrl,
        title,
        servings,
        vegan,
        vegetarian,
        gluten_free
      } = response.data;

      console.log(analyze_Instructions);

      let _instructions = analyze_Instructions
        .map((fstep) => {
          fstep.steps[0].step = fstep.name + fstep.steps[0].step;
          return fstep.steps;
        })
        .reduce((a, b) => [...a, ...b], []);
      
      let _recipe = {
        instructions,
        _instructions,
        analyze_Instructions,
        ingredients,
        popularity,
        readyInMinutes,
        imageUrl,
        title,
        servings,
        vegan,
        vegetarian,
        gluten_free
      };

      this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>
