<template>
<div>
  <i class="fa-regular fa-dice-d20"></i>
  <button class="btn btn-secondary" v-on:click="replaceRecipe(recipe.id)"></button>
  <img class="image" :src="require('../assets/meals/' + recipe.id + '.jpg')">
  <span class="title">{{recipe.name}}</span><br>
  <span class="description">{{recipe.description}}</span>
</div>
</template>

<script>

export default {
  name: "RecipeCard",
  props: ['recipe'],
  methods: {
    replaceRecipe(id){
      this.$parent.replaceRecipe(id);
    }
  }
}
</script>

<style scoped>
.image {
  border: #2c3e50 solid medium;
  border-radius: 5px;
  width: 300px;
  height: 200px;
  background-size: cover;
}
div {
  width: 310px;
  display: inline-block;
  margin: 15px;
  vertical-align: top;
}
.title{
  font-weight: bold;
  font-size: medium;
}
.description{
  font-size: small;
}
.btn{
  left: 8.52em;
  top: 2.1em;
  position: relative;
  background: #111111;
  background-image: url('../assets/Change.png');
  background-size: 20px, 30px;
  background-repeat: no-repeat;
  background-position: .1em .15em;
  width: 20px;
  height: 30px;
}
</style>