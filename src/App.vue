<template>
  <p>Value: {{ value }}</p>
  <vue-number-input v-model="value" :min="1" :max="30" @update:model-value="fetchData" inline controls></vue-number-input><br/>
  <RecipeCard v-for="recipe of recipes" :key="recipe.id" :recipe="recipe"/>
  <p v-if="loading">
    Still loading..
  </p>
  <p v-if="error">


  </p>
</template>

<script>
import RecipeCard from "@/components/RecipeCard";
import {ref} from "vue";
import { createApp } from 'vue';
import VueNumberInput from '@chenfengyuan/vue-number-input';

const app = createApp({});

app.component(VueNumberInput.name, VueNumberInput);

export default {
  data() {
    return {
      value: 1,
    };
  },
  name: 'App',
  components: {
    RecipeCard,
    VueNumberInput
  },
  methods: {
    replaceRecipe(id) {
      console.log(id);
      console.log(this.recipes.find(x => x.id === id));
      let clickedRecipe = this.recipes.find(x => x.id === id);
      const index = this.recipes.indexOf(clickedRecipe);

      fetch('https://veggieapi.azurewebsites.net/GetRandomMenu/1',  {
        method: 'get',
        headers: {
          'content-type': 'application/json'
        }
      })
          .then(res => {
            // a non-200 response code
            if (!res.ok) {
              // create error instance with HTTP status text
              const error = new Error(res.statusText);
              error.json = res.json();
              throw error;
            }
            return res.json();
          })
          .then(json => {
            // set the response data
            let x = json[0];
            this.recipes[index] = x;
            localStorage.recipes = JSON.stringify(this.recipes);
          })
          .catch(err => {
        this.error.value = err;
        // In case a custom JSON error response was provided
        if (err.json) {
          return err.json.then(json => {
            // set the JSON response message
            this.error.value.message = json.message;
          });
        }
      })

    },
    fetchData(newValue) {
      this.loading = true;
      // // I prefer to use fetch
      // // you can use use axios as an alternative
      return fetch('https://veggieapi.azurewebsites.net/GetRandomMenu/' + newValue, {
         method: 'get',
        headers: {
          'content-type': 'application/json'
        }
      })
          .then(res => {
            // a non-200 response code
            if (!res.ok) {
              // create error instance with HTTP status text
              const error = new Error(res.statusText);
              error.json = res.json();
              throw error;
            }
            return res.json();
          })
          .then(json => {
            // set the response data
            let x = json;
            if(JSON.parse(localStorage.recipes)) {
              this.recipes = JSON.parse(localStorage.recipes);
            }
            //this.recipes = JSON.parse(localStorage.recipes);
            console.log("test");
            //console.log(localStorage.recipes);
            if(this.recipes) {
              this.recipes = [...this.recipes, ...x];
            }
            else {
              this.recipes = x;
            }
            this.recipes.length = newValue;
            localStorage.recipes = JSON.stringify(this.recipes);
            console.log(localStorage.recipes);
            console.log(this.recipes);
          })
          .catch(err => {
            console.log(err);
            this.error.value = err;
            // In case a custom JSON error response was provided
            if (err.json) {
              return err.json.then(json => {
                // set the JSON response message
                this.error.value.message = json.message;
              });
            }
          })
          .then(() => {
            this.loading = false;
          });
    }
  },
  setup() {
    const recipes = ref(null);
    const loading = ref(true);
    const error = ref(null);

    return {
      recipes,
      loading,
      error
    };
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
