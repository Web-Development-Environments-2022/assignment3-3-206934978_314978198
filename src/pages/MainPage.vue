<template>
  <!-- @submit.prevent="onRandonRecipes" -->
  
  <div class="general">
    <h1 class="title">Main Page</h1>
    <br/>
  
    <div class="row_container">
    
      <div class="column1">
        
        <RecipePreviewList
          title="Explore This Recipes"
          :log_in="Boolean($root.store.username)"
          class="RandomRecipes center"
        />
        <!-- <button type="submit" class="btn btn-dark" @click="onRandomRecipes">
          More Recipes
        </button> -->
      </div>

      <div class="column2">
      <br/>
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


.title {
  margin-top: 10px;
  text-size-adjust: 20px;
  margin-left: 800px;
  font-size: 70px;
  color:darkgreen;
}
.RandomRecipes {
  margin: 30px 0 30px;
}
.blur {
  -webkit-filter: blur(20px);
  filter: blur(2px);
}

.row_container {
  display: flex;
  margin-left: 450px;
}
.column1 {
  right: 0
}
</style>
