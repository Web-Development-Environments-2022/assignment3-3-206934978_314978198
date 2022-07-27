<template>
<div>
    <b-button v-b-modal="'add-new-recipe-modal'">Add New Recipe</b-button>
    <b-modal
        id="add-new-recipe-modal"
        ref="modal"
        title="Add New Recipe"
        @show="resetModal"
        @hidden="resetModal"
        @ok="handleOk"
      >Ceate You're New Recipe:
        <br/>
        <br/>
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

          <b-form-group label="Recipe's Servings: " label-for="servings-input">
            <b-form-input
              id="servings-input"
              v-model="$v.form.servings"
              type="number"
              min="1"
              :state="validateState('servings')"
            ></b-form-input>
            <b-form-invalid-feedback v-if="!$v.form.servings.required">
              Recipe's servings is required
            </b-form-invalid-feedback>
            <b-form-invalid-feedback v-if="!$v.form.servings.numeric">
              Recipe's servings should be numeric
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
              min="1"
              :state="validateState('readyInMinutes')"
            ></b-form-input>
            <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.required">
              Recipe's time is required
            </b-form-invalid-feedback>
            <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.numeric">
              Recipe's time should be numeric
            </b-form-invalid-feedback>
          </b-form-group>

          <b-form-group label="Vegan: " label-for="vegan-input">
            <b-form-checkbox
              switch
              class="mr-n2 mb-n1"
              id="vegan-input"
              v-model="$v.form.vegan"
            >
              <span class="sr-only"></span>
            </b-form-checkbox>
          </b-form-group>

          <b-form-group label="Vegetarian: " label-for="vegetarian-input">
            <b-form-checkbox
              switch
              class="mr-n2 mb-n1"
              id="vegetarian-input"
              v-model="$v.form.vegetarian"
            >
              <span class="sr-only"></span>
            </b-form-checkbox>
          </b-form-group>

          <b-form-group label="Gluten Free: " label-for="gluten_free-input">
            <b-form-checkbox
              switch
              class="mr-n2 mb-n1"
              id="gluten-free-input"
              v-model="$v.form.gluten_free"
            >
              <span class="sr-only"></span>
            </b-form-checkbox>
          </b-form-group>

          <b-form-group id="input-group-ingredients" label-cols-sm="3" label="Ingredients" laber-for="ingredients">
          </b-form-group>

        <b-form-input id="ingredients" v-model="$v.form.ingrediants.$model" type="text" :state="validateState('ingredients')"></b-form-input>
          <b-form-invalid-feedback v-if="!$v.form.ingredients.required">
            Recipe's ingredientds is required
          </b-form-invalid-feedback>
          
          <!-- <b-form-group
            :state="Boolean(form.ingrediants)"
            v-for="ingrediant in form.ingrediants"
            :key="ingrediant.value"
            invalid-feedback="*"
          >
            <b-form-input
              v-model="ingrediant.value"
              required
              type="text"
            ></b-form-input>
          </b-form-group>

          <b-button-group>
            <b-button
              type="button"
              variant="outline-info"
              class="mb-2"
              @click="addInstruction"
            >
              <b-icon icon="plus-lg" aria-hidden="true"></b-icon>
            </b-button>
            <b-button
              type="button"
              variant="outline-info"
              class="mb-2"
              @click="removeInstruction"
            >
              <b-icon icon="dash-lg" aria-hidden="true"></b-icon>
            </b-button>
          </b-button-group> -->

          <!-- <b-form-group
            label="Recipe's Instractions: "
            label-for="instractions-input"
            v-for="instruc in form.instractions"
            :key="instruc.value"
            invalid-feedback="*"
          >
            <b-form-input
              id="instractions-input"
              v-model="instruc.value"
              type="text"
              :state="validateState('instractions')"
            ></b-form-input>
            <b-form-invalid-feedback v-if="!instruc.value.required">
              Recipe's instractions is required
            </b-form-invalid-feedback>
          </b-form-group> -->
          <!-- 
          <b-button-group>
            <b-button
              type="button"
              variant="outline-info"
              class="mb-2"
              @click="addInstruction"
            >
              <b-icon icon="plus-lg" aria-hidden="true"></b-icon>
            </b-button>
            <b-button
              type="button"
              variant="outline-info"
              class="mb-2"
              @click="removeInstruction"
            >
              <b-icon icon="dash-lg" aria-hidden="true"></b-icon>
            </b-button>
          </b-button-group> -->
        </b-form>
      </b-modal>
    </div>
</template>

<script>
    import { required, alpha,numeric } from "vuelidate/lib/validators";

    export default {
        name: 'NewRecipe',
        data() {
            return {
                form: {
                    title: "",
                    image: "",
                    servings: "",
                    readyInMinutes: "",
                    vegan: 0,
                    vegetarian: 0,
                    gluten_free: 0,
                    ingrediants: [{ key: 0, value: undefined }],
                    instractions: [{ key: 0, value: undefined }],
                    submitError: undefined,
                },
                ingrediantsCounter: 1,
                instructionsCounter: 1,
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
                servings: {
                    required,
                    numeric,
                },
                readyInMinutes: {
                    required,
                    numeric,
                },
                ingrediants: {
                    required,
                },
                instractions: {
                    required,
                },
            },
        },
        methods: {
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
                    servings: "",
                    readyInMinutes: "",
                    vegan: 0,
                    vegetarian: 0,
                    gluten_free: 0,
                    ingrediants: "",
                    instractions: "",
                };
                this.$nextTick(() => {
                    this.$v.$reset();
                });
            },

            handleOk(bvModalEvent) {
                // Prevent modal from closing
                bvModalEvent.preventDefault();
                // Trigger submit handler
                this.$v.form.$touch();
                if (this.$v.form.$anyError) {
                    return;
                }
                this.handleSubmit();
            },

            async createNewRecipe() {
            try {
                const response = await this.axios.post(
                this.$root.store.server_domain + "/user/myRecipies",
                {
                    title: this.form.title,
                    imageUrl: this.form.image,
                    readyInMinutes: this.form.readyInMinutes,
                    vegan: this.form.vegan,
                    vegetarian: this.form.vegetarian,
                    gluten_free: this.form.gluten_free,
                    ingredients: this.form.ingredients,
                    instructions: this.form.instructions,
                    servings: this.form.servings,
                    popularity: 0,
                }
                );
                if (response.status == 201) {
                this.$root.toast(
                    "Create New Recipe",
                    "New recipe added successfully",
                    "success"
                );
                }
            } catch (err) {
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

        addIngredient() {
            this.ingrediantsCounter += 1;

            this.form.ingrediants.push({
                key: this.ingrediantsCounter,
                value: undefined,
            });
        },

        addInstruction() {
            this.instructionsCounter += 1;

            this.form.instractions.push({
                key: this.instructionsCounter,
                value: undefined,
            });
        },

        removeIngredient() {
            if (this.ingrediantsCounter > 1) {
                this.ingrediantsCounter -= 1;
            }
            this.form.ingrediants.pop();
        },

        removeInstruction() {
            if (this.instructionsCounter > 1) {
                this.instructionsCounter -= 1;
            }
            this.form.instractions.pop();
            },
        }
    }
</script>

<style scoped>

</style>
