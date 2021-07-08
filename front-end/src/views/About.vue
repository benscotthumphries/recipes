<template>
  <div class="about">
    <button type="button" class="button btn btn-secondary" data-toggle="modal" data-target=".bd-example-modal-lg">New Recipe</button>
    <div class="modal fade bd-example-modal-lg" tabindex="-1" role="dialog" aria-labelledby="myLargeModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <form class='new'>
            <div class="form-group">
              <label class='label' for="formGroupExampleInput">Title:</label>
              <input v-model='title' type="text" class="form-control" id="formGroupExampleInput" placeholder="Cosmic Brownies">
            </div>
            <div class="form-group">
              <label class='label' for="exampleFormControlTextarea1">Ingredients:</label>
              <textarea v-model='ingredients' class="form-control" id="exampleFormControlTextarea1" placeholder="-Chocolate" rows="3"></textarea>
            </div>
            <div class="form-group">
              <label class='label' for="exampleFormControlTextarea2">Directions:</label>
              <textarea v-model='direction' class="form-control" id="exampleFormControlTextarea2" placeholder="Mix together..." rows="3"></textarea>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
              <button @click='save' type="button" class="btn btn-primary">Save</button>
            </div>
          </form>
        </div>
      </div>
    </div>

    <div class="container cont">
        <div class="row">
          <div class='card-columns'>
          <div class='cardbox' v-for="item in current" :key="item.id">
            <div class="card" style="width: 20rem;">
              <img v-bind:src="item.path">
              <div class="card-body">
                <h5 class="card-title">{{item.title}} </h5>
                <div class="card-text" v-for="index in item.ingredients" v-bind:key="index._id">
                  <p>{{index}}</p> 
                </div>
                <p class="card-title">{{item.direction}} </p>
                <ul class="list-group list-group-flush">
                  <p class="card-title"><a>{{item.path}} </a></p>
                </ul>
                
                <button v-if='!item.path' @click='saveInfo(item)' type="button" class="btn btn-secondary" data-toggle="modal" data-target=".bd-example-modal-l">Edit</button>
                  <div class="test modal fade bd-example-modal-l" tabindex="-1" role="dialog" aria-labelledby="myLargeModalLabel" aria-hidden="true">
                    <div class="modal-dialog modal-l">
                      <div class="modal-content">
                        <form class='new'>
                          <div class="form-group">
                            <label class='label' for="formGroupExampleInput">Title:</label>
                            <input v-model='editRecipe.title' type="text" class="form-control" id="formGroupExampleInput" placeholder="Cosmic Brownies">
                          </div>
                          <div class="form-group">
                            <label class='label' for="exampleFormControlTextarea1">Ingredients:</label>
                            <textarea v-model='editRecipe.ingredients' class="form-control" id="exampleFormControlTextarea1" placeholder="-Chocolate" rows="3"></textarea>
                          </div>
                          <div class="form-group">
                            <label class='label' for="exampleFormControlTextarea2">Directions:</label>
                            <textarea v-model='editRecipe.direction' class="form-control" id="exampleFormControlTextarea2" placeholder="Mix together..." rows="3"></textarea>
                          </div>
                          <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                            <button @click="update(editRecipe)" type="button"  class="btn btn-primary">Save</button>
                          </div>
                        </form>
                      </div>
                    </div>
                  </div>
                <button type="button" class="btn btn-secondary" @click="DeleteRecipe(item)">Delete</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'about',
  data() {
    return {
      current: {},
      title: '',
      ingredients: '',
      direction: '',
      editRecipe: {},
      newRecipe: {},
    }
  },
  methods: {
    async GetMyRecipies() {
      try {
        this.response = await axios.get("/api/recipes");
        this.current = this.response.data;
      } catch (error) {
        this.error = error.response.data.message;
      }
    },
    async DeleteRecipe(recipe) {
      try {
        await axios.delete("/api/recipes/" + recipe._id);
        this.GetMyRecipies();
        console.log(recipe)
      } catch (error) {
        console.log(error);
      }
    },
    save() {
      try {
        this.newRecipe.title = this.title;
        this.newRecipe.ingredients = this.ingredients;
        this.newRecipe.direction = this.direction;
        axios.post("/api/recipes", this.newRecipe);
      } catch (error) {
        this.error = "Error: " + error.response.data.message;
      }
      location.reload();
      
    },
    update(recipe){
      try {
        console.log(recipe)
        axios.put("/api/recipes/" + recipe._id, {
          title: recipe.title,
          ingredients: recipe.ingredients,
          direction: recipe.direction,
        });
        this.GetMyRecipies();
      } catch (error) {
        console.log(error);
      }
      location.reload();
    },
    saveInfo(recipe){
      console.log(recipe);
      this.editRecipe = recipe;
    },
    fileChanged(event) {
      this.file = event.target.files[0];
      this.url = URL.createObjectURL(this.file);
    },
    close() {
      this.$emit('close');
    },
    chooseImage() {
      this.$refs.fileInput.click()
    },
  },
  created() {
    this.GetMyRecipies();
  }

  
}
</script>

<style scoped>
.new{
  padding: 50px;
}
.label{
  display: flex;
  text-align: left;
}
p{
  text-align: left;
  white-space: pre-wrap;
}
.button{
  margin:20px;
  margin-right: 55px;
  float:right
}
img{
  width:300px;
}
.cont{
  margin-top: 20px;
}
</style>
