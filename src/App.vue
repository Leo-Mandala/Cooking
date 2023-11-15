<template>
  <div>
    <main>
      <div class="HeaderB">
        <h1>Bienvenue sur Bakerizz où vous pouvez trouver les meilleures recettes</h1>
      </div>
      <UserAuth @user-authenticated="setUserConnected" />
      <div v-for="recipe in Recipes" :key="recipe.id" class="boxx">
        <h2>{{ recipe.title }}</h2>
        <p>Ingredients: {{ recipe.ingredient.join(', ') }}</p>
        <p>Difficulté: {{ recipe.difficulty }}</p>
        <div class="image-container" v-if="recipe.Picture || recipe.pictureUrl">
          <img :src="recipe.Picture || recipe.pictureUrl" alt="Food Pic" class="recipe-image" />
        </div>
        <div v-if="userConnected" class="button-container">
          <button @click="toggleForm(recipe)" class="update-button">Update Recipe</button>
          <UpdateRecipe v-if="displayForm && selectedRecipe && selectedRecipe.id === recipe.id" :recipe="recipe" @update-recipe="handleRecipeUpdate" />
        </div>
        <div v-else class="button-container">
          <button @click="handleAlert" class="update-button">Update Recipe</button>
        </div>
      </div>
      <div v-if="userConnected" class="button-container">
        <br>
        <button @click="toggleForm(null)" class="add-button">Add Recipe</button>
        <AddRecipe v-if="displayForm && !selectedRecipe" @add-recipe="handleAddRecipe" />
      </div>
      <div v-else class="button-container">
        <button @click="handleAlert" class="add-button">Add Recipe</button>
      </div>
    </main>
    <footer>
      <h4>Contactez-moi ici :</h4>
      <ul class="contact-list">
  <li>
    <div class="logo-container">
      <a href="https://www.instagram.com"><img src="./assets/instagram_icon.png" alt="" class="logo" /></a>
    </div>
  </li>
  <li>
    <div class="logo-container">
      <img src="./assets/linkedin-icon-2048x2048-ya5g47j2.png" alt="" class="logo" />
    </div>
  </li>
  <br />
  <li>mail: leo.mandala@outlook.fr</li>
</ul>

    </footer>
  </div>
</template>


<script>
import { RouterLink, RouterView } from 'vue-router';
import UpdateRecipe from './components/ReciepeUpdate.vue';
import AddRecipe from './components/AddRecipe.vue';
import UserAuth from './components/UserAuth.vue';

export default {
  name: 'App',
  components: {
    UpdateRecipe,
    AddRecipe,
    UserAuth,
  },
  data() {
    return {
      Recipes: [
        {
          id: 1,
          title: 'tarte au chocolat',
          ingredient: ['chocolat', 'beurre', 'farine'],
          difficulty: 'low',
          Picture: null,
          pictureUrl: 'https://empreintesucree.fr/wp-content/uploads/2020/05/1-tarte-chocolat-recette-empreinte-sucree-1.jpg'
        },
        {
          id: 2,
          title: 'tarte aux pommes',
          ingredient: ['pomme', 'beurre', 'farine'],
          difficulty: 'low',
          Picture: null,
          pictureUrl: 'https://static.750g.com/images/1200-675/a96d46e59b4f0ab8169c7cb0cb932a84/la-cuisson.jpg'
        },
      ],
      userConnected: false,
      displayForm: false,
      selectedRecipe: null,
    };
  },
  methods: {
    handleRecipeUpdate(updatedRecipe) {
      if (this.userConnected) {
        const index = this.Recipes.findIndex((recipe) => recipe.id === updatedRecipe.id);
        if (index !== -1) {
          this.Recipes.splice(index, 1, updatedRecipe);
          this.Recipes = [...this.Recipes];
        }
      } else {
        alert('Please connect to update a recipe.');
        console.log('User not connected. Cannot update recipe.');
      }
    },
    handleAddRecipe(newRecipe) {
      if (this.userConnected) {
        this.Recipes.push(newRecipe);
      } else {
        alert('Please connect to add a recipe.');
        console.log('User not connected. Cannot add recipe.');
      }
    },
    setUserConnected(value) {
      this.userConnected = value;
    },
    toggleForm(recipe) {
      if (this.userConnected) {
        this.displayForm = !this.displayForm;
        this.selectedRecipe = recipe || null; 
      } else {
        this.handleAlert();
      }
    },
    
    handleAlert() {
      alert('Please connect to add or update a recipe.');
      console.log('User not connected. Cannot access form.');
    },
  },
};

</script>

<style>
body {
  background-color: #f4f4f4; /* Light gray background */
  font-family: 'Roboto', sans-serif; /* Use a Google Font for a modern look */
  margin: 0;
}

main {
  max-width: 800px;
  margin: 0 auto;
}

.HeaderB {
  background-color: #ff6f61; /* Marmiton's red color */
  color: white;
  padding: 20px;
  text-align: center;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.boxx {
  background-color: #fff;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.image-container {
  text-align: center;
  margin: 20px 0;
}

.recipe-image {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.button-container {
  text-align: center;
  margin-top: 20px;
}

.update-button,
.add-button {
  border-radius: 8px;
  background-color: #009688; /* Marmiton's green color */
  border: none;
  text-decoration: none;
  display: inline-block;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.3);
  font-size: 15px;
  padding: 10px 20px;
  margin-right: 10px;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.update-button:hover,
.add-button:hover {
  background-color: #00796b; /* Darker green on hover */
}

footer {
  background-color: #2c3e50; /* Dark blue for the footer */
  color: white;
  padding: 20px;
  text-align: center;
}

.contact-list {
  display: flex;
  justify-content: center;
  padding: 0;
}

.contact-list li {
  list-style: none;
  margin-right: 20px;
}

.logo-container {
  display: flex;
  justify-content: center;
}

.logo {
  width: 40px;
  height: auto;
  margin: 10px;
  border-radius: 50%;
}

@media only screen and (max-width: 600px) {
  .logo {
    width: 30px;
  }
}

@media only screen and (min-width: 600px) {
  .logo {
    width: 40px;
  }
}

@media only screen and (min-width: 768px) {
  .logo {
    width: 50px;
  }
}

@media only screen and (min-width: 992px) {
  .logo {
    width: 60px;
  }
}

@media only screen and (min-width: 1200px) {
  .logo {
    width: 70px;
  }
}
</style>
