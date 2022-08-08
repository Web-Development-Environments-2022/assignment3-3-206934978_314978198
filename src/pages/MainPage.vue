<template>
  <div class="container" @submit.prevent="onRandonRecipes">
    <h1 class="title">Main Page</h1>
    <div id="rightRecipes">
      <RecipePreviewList
        title="Explore This Recipes"
        class="RandomRecipes center"
      />
    </div>

    <div id="leftRecipes">
      <RecipePreviewList
        v-if="$root.store.username"
        title="Last Viewed Recipes"
        :log_in = "Boolean($root.store.username)"
        :class="{
          RandomRecipes: true,
          blur: !$root.store.username,
          center: true,
        }"
        disabled
      ></RecipePreviewList>
    </div>

    <!-- <button type="submit" class="btn btn-dark">More Recipes</button>
    <router-link v-if="!$root.store.username" to="/login" tag="button"
      >You need to Login to vue this</router-link
    > -->
    <!-- {{ !$root.store.username }} -->

    <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
    </div>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList.vue";
export default {
  components: {
    RecipePreviewList,
  },

  // methods: {
  //   onRandonRecipes() {},
  // },
};
</script>

<style lang="scss" scoped>
.conteiner {
  display: inline;
}
#rightRecipes{
  float: right,
}
#leftRecipes{
  float: left,
}
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
</style>
