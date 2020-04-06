<template>
  <v-timeline align-top :dense="$vuetify.breakpoint.smAndDown" class="pa-5 ma-5">
    <v-timeline-item
      v-for="(item, i) in items"
      :key="i"
      :color="item.color"
      :icon="item.icon"
      fill-dot
    >
      <v-card
        :color="item.color"
        dark
      >
        <v-card-title class="title">{{item.title}}</v-card-title>
        <v-card-text class="white text--primary pa-5">
          <p>{{item.text}}</p>
        </v-card-text>
      </v-card>
    </v-timeline-item>
  </v-timeline>
</template>

<script>
  export default {
    data: () => ({
      items: [
        {
          color: 'light-blue darken-1',
          icon: 'mdi-github',
          title: 'Commit Viewer',
          text: 'Nessa página você pode carregar um repositório e ver informações do commit. O repositório padrão é o oficial do vue.js, mas é possível carregar qualquer um inserindo no formulário do Floating Action Button e apertando no botão Carregar.'
        },
        {
          color: 'light-blue darken-2',
          icon: 'mdi-food',
          title: 'Randow Meal Generator',
          text: 'Ao clicar no botão da maçã, você gera uma receita aleatória. É possível avaliar as receitas com estrelas, curtir e abrir um vídeo no youtube. Avaliações e curtidas são mostradas em gráficos na página de Dashboard.'
        },
        {
          color: 'light-blue darken-3',
          icon: 'mdi-monitor-dashboard',
          title: 'Dashboard',
          text: 'Gráficos referentes a interações com o site. Visualizações, curtidas e avaliações são resumoidas aqui.'
        },
        {
          color: 'light-blue darken-3',
          icon: 'mdi-account-key',
          title: 'Administrators',
          text: 'Simulando tela de administradores'
        }
      ],
    }),
  }
</script>

<style>

</style>