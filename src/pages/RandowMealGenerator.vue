<template>
  <v-container>
    <v-row no-gutters dense class="d-flex flex-column justify-center">
      <v-col align="center" justify="center">
        <v-card
          class="ma-10"
          max-width="500px"
          align="center"
          justify="center"
          flat
          v-show="!meal.idMeal"
        >
          <v-list-item three-line>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1 text-md-center">Procurando uma receita?</v-list-item-title>
              <v-list-item-subtitle>Clique no botão!</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
          <v-btn
            :loading="loading"
            :disabled="loading"
            color="green"
            class="mb-5 white--text"
            fab
            @click="buscaReceita"
          >
            <v-icon dark>mdi-food-apple</v-icon>
          </v-btn>
        </v-card>
      </v-col>
      <v-col>
        <v-card class="d-inline-block mx-auto pa-10" v-if="meal.idMeal">
          <v-container>
            <v-row no-gutters dense justify="space-between">
              <v-col class="mr-5">
                <v-img transition="scale-transition" origin="center center" max-width="400px" :src="meal.strMealThumb" />

                <p class="mt-5 subtitle-2">Categoria: {{meal.strCategory}}</p>
                <p class="mt-2 subtitle-2">Origem: {{meal.strArea}}</p>
                <p class="mt-2 subtitle-2" v-show="meal.strTags">Tags: {{meal.strTags}}</p>

                <p class="mt-5 subtitle-2">Ingredientes</p>
                <ul>
                  <li
                    class="body-1"
                    v-for="ingredient in meal.ingredients"
                    :key="ingredient"
                  >{{ingredient}}</li>
                </ul>
                <p></p>
              </v-col>
              <v-col class="mr-5">
                <v-card-title primary-title>
                  <h3 class="headline">{{meal.strMeal}}</h3>
                </v-card-title>
                <v-card-actions class="mb-4">
                  <span class="grey--text text--lighten-2 caption mr-2">({{ rating }})</span>
                  <v-rating          
                    v-model="rating"
                    @input="ratingMeal"
                    background-color="yellow"
                    color="yellow accent-4"
                    dense
                    half-increments
                    hover
                    size="25"
                  ></v-rating>
                </v-card-actions>

                <p>{{meal.strInstructions}}</p>
              </v-col>
              <v-col cols="auto" class="text-center pl-0">
                <v-row class="flex-column ma-0" justify="center">
                  <v-col class="px-0">
                    <v-btn icon @click="saveMeal">
                      <v-icon :color="save ? 'red' : '' ">mdi-heart</v-icon>
                    </v-btn>
                  </v-col>

                  <v-col class="px-0">
                    <v-btn icon :href="meal.strYoutube" target="_blanck">
                      <v-icon>mdi-youtube</v-icon>
                    </v-btn>
                  </v-col>

                  <v-col class="px-0">
                    <v-btn icon>
                      <v-icon>mdi-share-variant</v-icon>
                    </v-btn>
                  </v-col>
                </v-row>
              </v-col>
            </v-row>
          </v-container>
        </v-card>
      </v-col>
    </v-row>
    <v-btn
      :loading="loading"
      :disabled="loading"
      color="green"
      class="mb-5 white--text"
      fab
      bottom
      fixed
      right
      @click="buscaReceita"
      v-show="meal.idMeal"
    >
      <v-icon dark>mdi-food-apple</v-icon>
    </v-btn>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      save: false,
      loading: false,
      rating: 0,
      meal: {
        idMeal: 0,
        strMeal: "",
        strCategory: "",
        strArea: "",
        strInstructions: "",
        strMealThumb: "",
        strTags: "",
        strYoutube: "",
        ingredients: []
      }
    };
  },
  methods: {
    saveMeal() {
      this.save = !this.save;
      if(this.save){
        this.$emit("registerLog", {category: 'like-meal', value: this.meal})
      }
    },
    ratingMeal() {
      this.$emit("registerLog", {category: 'rating-meal', value: this.meal, rating: this.rating})
    },
    buscaReceita() {
      this.loading = true;
      this.save = false;
      var url = "https://www.themealdb.com/api/json/v1/1/random.php"; //Sua URL
      var xhttp = new XMLHttpRequest();
      xhttp.open("GET", url, true);
      const instancia = this;
      xhttp.onreadystatechange = function() {
        if (xhttp.readyState == 4 && xhttp.status == 200) {
          instancia.loading = false;
          let obj = JSON.parse(xhttp.responseText);
          instancia.meal = obj.meals[0];
          instancia.meal.ingredients = [];
          for (let i = 1; i <= 20; i++) {
            if (instancia.meal[`strIngredient${i}`]) {
              instancia.meal.ingredients.push(
                `${instancia.meal[`strIngredient${i}`]} - ${
                  instancia.meal[`strMeasure${i}`]
                }`
              );
            } else {
              break;
            }
          }
          //instancia.rating = Number(((Math.random() * (6 - 2) + 2)).toFixed(1));
          instancia.rating = 0;
        }
      };

      xhttp.send();
    }
  }
};

</script>
