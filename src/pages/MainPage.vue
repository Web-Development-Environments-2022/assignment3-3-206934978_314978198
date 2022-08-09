<template>
  <!-- @submit.prevent="onRandonRecipes" -->
  <div class="container">
    <!-- <h1 class="title">Main Page</h1> -->
    <div class="right-div">
      <RecipePreviewList
        title="Explore This Recipes"
        :log_in="Boolean($root.store.username)"
        class="RandomRecipes center"
      />
      <!-- <button type="submit" class="btn btn-dark" @click="onRandomRecipes">
        More Recipes
      </button> -->
    </div>

    <div class="left-div">
      <RecipePreviewList
        v-if="$root.store.username"
        title="Last Viewed Recipes"
        :log_in="Boolean($root.store.username)"
        :class="{
          RandomRecipes: true,
          center: true,
        }"
        disabled
      ></RecipePreviewList>
      <LogInMainPage v-else></LogInMainPage>
    </div>

    <div
      style="
        position: absolute;
        top: 70%;
        left: 50%;
        transform: translate(-50%, -50%);
      "
    ></div>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList.vue";
import LogInMainPage from "../components/LogInMainPage.vue";
export default {
  components: {
    RecipePreviewList,
    LogInMainPage,
  },
  data() {
    return {
      rerandom: false,
    };
  },

  methods: {
    onRandonRecipes() {
      if (this.rerandom) {
        this.rerandom = false;
      } else {
        this.rerandom = true;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
// .conteiner {
//   display: inline;
// }

.right-div {
  float: right;
}

.left-div {
  float: left;
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
