<template>
<div>
  <img class="image" :src="require('../assets/meals/' + recipe.id + '.jpg')">
  <span class="title">{{recipe.name}}</span><br>
  <span class="description">{{recipe.description}}</span>
</div>
</template>

<script>
export default {
  name: "RecipeCard",
  props: ['recipe']
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
</style>