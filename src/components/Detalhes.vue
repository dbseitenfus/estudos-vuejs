<template>
  <v-card class="mx-auto" max-width="344">
    <v-img :src="detalhe.author.avatar_url" height="200px"></v-img>

    <v-card-title>{{detalhe.commit.author.name}}</v-card-title>

    <v-card-subtitle>{{detalhe.commit.author.email}}</v-card-subtitle>

    <v-card-actions>
      <v-btn color="purple" text :href="detalhe.author.html_url" target="_blank">Acessar GitHub</v-btn>

      <v-spacer></v-spacer>

      <v-btn icon @click="show = !show">
        <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
      </v-btn>
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>

        <v-card-text>Mensagem: {{detalhe.commit.message}}<br> Data: {{detalhe.commit.author.date}}</v-card-text>
      </div>
    </v-expand-transition>
  </v-card>

  <!-- <v-card class="d-flex flex-column mb-6 justify-center ma-3" color="white" max-width="350px">
    <v-card-text>
      <div class="circle">
        <v-img
          :src="detalhe.author.avatar_url"
          lazy-src="../assets/loading_image.jpg"
          aspect-ratio="1"
          class="grey lighten-2 redonda"
          max-width="500"
          max-height="300"
        >
          <template v-slot:placeholder>
            <v-row class="fill-height ma-0" align="center" justify="center">
              <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
            </v-row>
          </template>
        </v-img>
      </div>
      <h2 class="mb-5">Autor: {{detalhe.commit.author.name}}</h2>
      <h2 class="mb-5">Mensagem: {{detalhe.commit.message}}</h2>
      <h2 class="mb-5">Data: {{detalhe.commit.author.date}}</h2>
    </v-card-text>
  </v-card>-->
</template>

<script>
export default {
  data: () => ({
    show: false
  }),
  props: {
    detalhe: {
      type: Object
    }
  }
};
</script>

<style scoped>
.circle {
  background-color: #aaa;
  border-radius: 50%;
  width: 150px;
  height: 150px;
  overflow: hidden;
  position: relative;
}

.circle .redonda {
  position: absolute;
  bottom: 0;
  width: 100%;
}
</style>