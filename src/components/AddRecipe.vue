<template>
  <div>
    <h1>ajouter une recette</h1>
    <form @submit="addRecipe">
      <div>
        <label for="newTitle">Titre:</label>
        <input type="text" v-model="newRecipe.title" name="newTitle" />
      </div>
      <div>
        <label for="newIngredients">Ingredients:</label>
        <input type="text" v-model="newRecipe.ingredient" name="newIngredients" />
      </div>
      <div>
        <label for="newDifficulty">Difficulté:</label>
        <input type="text" v-model="newRecipe.difficulty" name="newDifficulty" />
      </div>
      <div>
        <label for="newPicture">Photo:</label>
        <input type="file" accept="image/*" @change="onFileChange" />
      </div>
      <div>
        <label for="newPictureUrl">URL de l'image:</label>
        <input type="text" v-model="newRecipe.pictureUrl" name="newPictureUrl" />
      </div>
      <div v-if="newRecipe.Picture">
        <img :src="newRecipe.Picture" alt="Recipe Picture" />
      </div>
      <div v-else-if="newRecipe.pictureUrl">
        <img :src="newRecipe.pictureUrl" alt="Recipe Picture" />
      </div>
      <button type="submit">Add Recipe</button>
    </form>
  </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';

export default {
  name: 'AddRecipe',
  data() {
    return {
      newRecipe: {
        title: '',
        ingredient: '',
        difficulty: '',
        Picture: null,
        pictureUrl: ''
      },
    };
  },
  methods: {
    addRecipe(event) {
      event.preventDefault();
      const id = uuidv4();

      this.newRecipe.ingredient = this.newRecipe.ingredient.split(', ');

      this.newRecipe.id = id;

      this.$emit('add-recipe', this.newRecipe);

      this.newRecipe = {
        title: '',
        ingredient: '',
        difficulty: '',
        Picture: null,
        pictureUrl: ''
      };
    },
    onFileChange(event) {
      const file = event.target.files[0];
      this.newRecipe.Picture = URL.createObjectURL(file);
    }
  }
};
</script>

<style scoped>
input[type=text] {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
}
</style>
