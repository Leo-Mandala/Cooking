<script>
export default {
  name: 'UpdateRecipe',
  props: {
    recipe: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      updatedRecipe: {
        id: this.recipe.id,
        title: this.recipe.title,
        ingredient: this.recipe.ingredient.join(', '),
        difficulty: this.recipe.difficulty,
        Picture: this.recipe.Picture, 
        pictureUrl: this.recipe.pictureUrl, 
      },
    };
  },
  methods: {
    updateRecipe() {
      const updatedFields = {
        id: this.updatedRecipe.id,
        title: this.updatedRecipe.title,
        ingredient: this.updatedRecipe.ingredient.split(', '),
        difficulty: this.updatedRecipe.difficulty,
        Picture: this.updatedRecipe.Picture, 
        pictureUrl: this.updatedRecipe.pictureUrl, 
      };
      this.$emit('update-recipe', updatedFields);
    },
    onFileChange(event) {
      const file = event.target.files[0];
      const reader = new FileReader();

      reader.onload = (e) => {
        this.updatedRecipe.Picture = e.target.result;
      };

      reader.readAsDataURL(file);
    },
  },
};
</script>

<template>
  <div>
    <h2>Metter a jour la recette</h2>
    <form @submit.prevent="updateRecipe">
      <div>
        <label for="title">Title:</label>
        <input type="text" id="title" v-model="updatedRecipe.title" />
      </div>
      <div>
        <label for="ingredients">Ingredients:</label>
        <input type="text" id="ingredients" v-model="updatedRecipe.ingredient" />
      </div>
      <div>
        <label for="difficulty">Difficulty:</label>
        <input type="text" id="difficulty" v-model="updatedRecipe.difficulty" />
      </div>
      <div>
        <label for="Picture">Picture:</label>
        <input type="file" accept="image/*" @change="onFileChange" />
      </div>
      <div v-if="updatedRecipe.Picture">
        <img :src="updatedRecipe.Picture" alt="Recipe Picture" />
      </div>
      <div>
        <label for="pictureUrl">Picture URL:</label>
        <input type="text" id="pictureUrl" v-model="updatedRecipe.pictureUrl" />
      </div>
      <button type="submit">Mettre a jour</button>
    </form>
  </div>
</template>

<style scoped>
input[type=text] {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
}
</style>
