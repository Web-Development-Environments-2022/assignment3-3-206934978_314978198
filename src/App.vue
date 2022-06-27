<template>
  <div id="app">
    <div id="nav">
      <router-link :to="{ name: 'main' }">Vue Recipes</router-link>|
      <router-link :to="{ name: 'search' }">Search</router-link>|
      {{ !$root.store.username }}
      <span v-if="!$root.store.username">
        Guest:
        <router-link :to="{ name: 'register' }">Register</router-link>|
        <router-link :to="{ name: 'login' }">Login</router-link>|
      </span>
      <span v-else>
        {{ $root.store.username }}: <button @click="Logout">Logout</button>|
      </span>
      <b-button v-b-modal.modal-prevent-closing>Add New Recipe</b-button>

      <b-modal
        id="modal-prevent-closing"
        ref="modal"
        title="Add New Recipe"
        @show="resetModal"
        @hidden="resetModal"
        @ok="handleOk"
      >
        <b-form ref="form" @submit.stop.prevent="handleSubmit">
          <b-form-group label="Recipe's Title: " label-for="title-input">
            <b-form-input
              id="title-input"
              v-model="$v.form.title.$model"
              type="text"
              :state="validateState('title')"
            ></b-form-input>
            <b-form-invalid-feedback v-if="!$v.form.title.required">
              recipes's title is required
            </b-form-invalid-feedback>
            <b-form-invalid-feedback v-if="!$v.form.title.alpha">
              Recipce's title can only contain letters
            </b-form-invalid-feedback>
          </b-form-group>

          <b-form-group label="Recipe's image URL: " label-for="image-input">
            <b-form-input
              id="image-input"
              v-model="$v.form.image.$model"
              type="url"
              :state="validateState('image')"
            ></b-form-input>
            <b-form-invalid-feedback v-if="!$v.form.title.required">
              Recipe's image URL is required
            </b-form-invalid-feedback>
          </b-form-group>

          <b-form-group label="Recipe's Survings: " label-for="survings-input">
            <b-form-input
              id="survings-input"
              v-model="$v.form.survings.$model"
              type="number"
              :state="validateState('survings')"
            ></b-form-input>
            <b-form-invalid-feedback v-if="!$v.form.survings.required">
              Recipe's survings is required
            </b-form-invalid-feedback>
            <b-form-invalid-feedback v-if="$v.form.survings.alpha">
              Recipe's survings can only contain numbers
            </b-form-invalid-feedback>
          </b-form-group>

          <b-form-group
            label="Recipe's Time: "
            label-for="readyInMinutes-input"
          >
            <b-form-input
              id="readyInMinutes-input"
              v-model="$v.form.readyInMinutes.$model"
              type="number"
              :state="validateState('readyInMinutes')"
            ></b-form-input>
            <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.required">
              Recipe's time is required
            </b-form-invalid-feedback>
            <b-form-invalid-feedback v-if="$v.form.readyInMinutes.alpha">
              Recipe's time can only contain numbers
            </b-form-invalid-feedback>
          </b-form-group>

          <b-form-group label="Vegan: " label-for="vegan-input">
            <b-form-checkbox switch class="mr-n2 mb-n1">
              <span class="sr-only"></span>
            </b-form-checkbox>
          </b-form-group>

          <b-form-group label="Vegetarian: " label-for="vegetarian-input">
            <b-form-checkbox switch class="mr-n2 mb-n1">
              <span class="sr-only"></span>
            </b-form-checkbox>
          </b-form-group>

          <b-form-group label="Gluten Free: " label-for="gluten_free-input">
            <b-form-checkbox switch class="mr-n2 mb-n1">
              <span class="sr-only"></span>
            </b-form-checkbox>
          </b-form-group>

          <b-form-group
            label="Recipe's Ingrediants: "
            label-for="ingrediants-input"
          >
            <b-form-input
              id="ingrediants-input"
              v-model="$v.form.ingrediants.$model"
              type="text"
              :state="validateState('ingrediants')"
            ></b-form-input>
            <b-form-invalid-feedback v-if="!$v.form.ingrediants.required">
              Recipe's ingrediants is required
            </b-form-invalid-feedback>
          </b-form-group>
          <!-- <b-button v-b-modal.modal-prevent-closing >Add ingrediants</b-button> -->
          <b-form-group
            label="Recipe's Instractions: "
            label-for="instractions-input"
          >
            <b-form-input
              id="instractions-input"
              v-model="$v.form.instractions.$model"
              type="text"
              :state="validateState('instractions')"
            ></b-form-input>
            <b-form-invalid-feedback v-if="!$v.form.instractions.required">
              Recipe's instractions is required
            </b-form-invalid-feedback>
          </b-form-group>
        </b-form>
      </b-modal>
    </div>
    <router-view />
  </div>
</template>

<script>
import { required, alpha } from "vuelidate/lib/validators";

export default {
  name: "App",

  data() {
    return {
      form: {
        title: "",
        image: "",
        survings: "",
        readyInMinutes: "",
        vegan: "",
        vegetarian: "",
        gluten_free: "",
        ingrediants: "",
        instractions: "",
        submitError: undefined
      },
    };
  },

  validations: {
    form: {
      title: {
        required,
        alpha,
      },
      image: {
        required,
      },
      survings: {
        required,
        alpha,
      },
      readyInMinutes: {
        required,
        alpha,
      },
      ingrediants: {
        required
      },
      instractions: {
        required
      },
    },
  },
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },

    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },

    // checkFormValidity() {
    //   const valid = this.$refs.form.checkValidity();
    //   this.nameState = valid;
    //   return valid;
    // },

    resetModal() {
      this.form = {
        title: "",
        image: "",
        survings: "",
        readyInMinutes: "",
        vegan: "",
        vegetarian: "",
        gluten_free: "",
        ingrediants: "",
        instractions: "",
      }
      this.$nextTick(() => {
        this.$v.$reset();
      });
    },

    handleOk(bvModalEvent) {
      // Prevent modal from closing
      bvModalEvent.preventDefault();
      // Trigger submit handler
      this.handleSubmit();
    },

    async createNewRecipe(){
      try{
          const response = await this.axios.post(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          this.$root.store.server_domain + "/user/myRecipies",

          {
            title: this.form.title,
            imageUrl: this.form.image,
            readyInMinutes: this.form.readyInMinutes,
            vegan: this.form.vegan,
            gluten_free: this.form.gluten_free,
            ingredients: this.form.ingredients,
            instructions: this.form.instructions,
            servings: this.form.servings,
            popularity: 0
          }
        );
        if (response.status == 201) {
          this.$root.toast("Create New Recipe", "New recipe added successfully", "success");
        }
      }
      catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },

    handleSubmit() {
      // Exit when the form isn't valid
      // if (!this.checkFormValidity()) {
      //   return;
      // }
      // Push the name to submitted names
      
      // Hide the modal manually
      this.createNewRecipe();
      this.$nextTick(() => {
        this.$bvModal.hide("modal-prevent-closing");
      });
    },
  },
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
