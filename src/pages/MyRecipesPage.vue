<template>
    <div>
        <b-container>
            <b-row>
                <b-col v-for="r in this.results" :key="r.id">
                <RecipePreview class="recipePreview" :recipe="r" />
                </b-col>
            </b-row>
        </b-container>
    </div>    
</template>



<script>
import { RecipePreview } from "../components/RecipePreview.vue";

export default{
    name: "MyRecipes",
    components:{
        RecipePreview,
    },
    data(){
        return{
            results: [],
        }
    },
    methods: {
        async Search() {
            try {
                const response = await this.axios.get();
                if (response.data === "There is no results!") {
                    this.$root.toast(
                    "My Recipes",
                    "There is no results",
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
                }

            }
            catch (err){
                console.log(err.response);
                this.submitError = err.response.data.message;
            }
        }
    }
}
</script>



<style scoped>

</style>
