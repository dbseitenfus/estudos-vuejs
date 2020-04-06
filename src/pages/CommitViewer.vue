<template>
  <v-container>
    <v-dialog v-model="dialog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">Repositório</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="6" md="4">
                <v-text-field label="User*" v-model="repos.user" required>{{repos.user}}</v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-text-field label="Repositório*" hint="example of helper text only on focus" v-model="repos.name">{{repos.name}}</v-text-field>
              </v-col>
             
            </v-row>
          </v-container>
          <small>*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialog = false">Close</v-btn>
          <v-btn color="blue darken-1" text @click="dialog = false">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-row no-gutters dense class="d-flex flex-row justify-center">
      <v-col>
        <Tabela @clique-tabela="detalhe = $event" :repos="repos"/>
      </v-col>
      <v-col>
        <Detalhes :detalhe="detalhe" />
      </v-col>
    </v-row>
    <v-btn
            color="blue"
            class="mb-5 white--text"
            fab
            right
            bottom
            fixed
            @click="dialog = true"
          >
            <v-icon dark>mdi-github</v-icon>
          </v-btn>
  </v-container>
</template>

<script>
import Tabela from "../components/Tabela";
import Detalhes from "../components/Detalhes";

export default {
  name: "CommitViewer",

  components: {
    Tabela,
    Detalhes
  },

  data: () => ({
    dialog: false,
    repos:{
      user: 'vuejs',
      name: 'vue'
    },
    detalhe: {
      commit: {
        author: {
          name: "",
          email: "",
          date: ""
        },
        committer: {
          name: "",
          email: "",
          date: ""
        },
        message: ""
      },
      author: {
        login: "",
        id: "",
        node_id: "",
        avatar_url: "",
        url: ""
      }
    }
  })
};
</script>
