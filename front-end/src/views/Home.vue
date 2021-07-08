<template>

  <div class="home">
    <div class='searchbar'>
        <input class="form-control"  type='search' v-model='search' placeholder="Search">
        <button @click="Searchrecipes()" class="btn btn-outline-success" >Search</button>
    </div>
    <div v-show='this.alert' class="alert alert-success" role="alert">
      Success!
    </div>
    <div class="container cont">
        <div class="row">
          <div class='card-columns'>
          <div class='cardbox' v-for="item in current.hits" v-bind:key="item.recipe.label">
            <div class="card" style="width: 20rem;">
              <img v-bind:src="item.recipe.image">
              <div class="card-body">
                <h5 class="card-title">{{item.recipe.label}} </h5>
                <div class="card-text" v-for="index in item.recipe.ingredientLines" v-bind:key="index">
                  <li>{{index}}</li> 
                </div>
              </div>
              <ul class="list-group list-group-flush">
                <li class="list-group-item">Calories: {{item.recipe.calories}}<button type="button" class="heart btn btn-outline-danger" @click="heart(item.recipe), toastr.success('Hi! I am success message.')" >Save</button></li>
              </ul>
              <ul class="list-group list-group-flush">
                <h5 class="card-title"><a>{{item.recipe.url}}</a></h5>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
// const apiURL = "https://api.edamam.com/search?q=";
// const apiKey = "&app_key=7779995897952ef60d87c354d1af516f";
// const apiId = "&app_id=29864b88";


export default {
  name: 'home',
  data() {
    return {
      current: {},
      search: '',
      newRecipe: {},
      alert: false,
    }
  },
  methods: {
    async Searchrecipes() {
      try {
        let url = 'https://api.edamam.com/api/recipes/v2?type=public&q=' + this.search + '&app_id=29864b88&app_key=7779995897952ef60d87c354d1af516f';
        const response = await axios.get(url);
        this.current = response.data;
        this.current.hits.forEach(a => a.recipe.calories = a.recipe.calories.toFixed(0));
        console.log(this.current)
      } catch (error) {
        console.log(error);
      }
    },
    heart(recipe) {
      console.log('sup')
      this.newRecipe.title = recipe.label;
      this.newRecipe.path = recipe.image;
      this.newRecipe.ingredients = recipe.ingredientLines;
      this.newRecipe.direction = recipe.direction;
      this.newRecipe.link = recipe.url;
      try {
        axios.post("/api/recipes", this.newRecipe);
        this.newRecipe = {};
      } catch (error) {
        this.error = "Error: " + error.response.data.message;
      }
      alert("Recipe has been added")
    }
  },
  created() {

  }
  
}


</script>

<style scoped>

.card-columns {
  column-count: 3;
  
}
.searchbar{
  display: flex;
  width: 100%;
  padding-top: 20px;
  padding-right: 300px;
  padding-bottom: 20px;
  padding-left: 300px;
}
.cont {
  margin: 0px;
  display: flex;
  /* align-items: center; */
  justify-content: center;
  width:100%;
}
li{
  text-align: left;
}
.heart {
  margin-left: 70px;
}


@media screen and (max-width: 992px) {
  .card-columns {
  column-count: 2;
}
.searchbar{
  padding-right: 100px;
  padding-left: 100px;
}
}

/* On screens that are 600px wide or less, make the columns stack on top of each other instead of next to each other */
@media screen and (max-width: 600px) {
  .card-columns {
  column-count: 1;
}
.searchbar{
  padding-right: 20px;
  padding-left: 20px;
}
}
</style>
