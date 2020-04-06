<template>
  <v-container max-width="700px">
    <v-card>
      <v-card-title>
        <v-text-field v-model="search" label="Search" single-line hide-details></v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="commits"
        :items-per-page="5"
        :search="search"
        class="elevation-1"
        :loading="carregando"
        loading-text="Carregando... Espera só um pouquinho :)"
        @click:row="cliqueLinha"
      >
        <template v-slot:top>
          <v-dialog v-model="dialog" max-width="500px">
            <v-card>
              <v-card-title>
                <span class="headline">Adicionar commit</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field v-model="editedCommit.commit.author.name" label="Autor"></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field v-model="editedCommit.commit.message" label="Mensagem"></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field v-model="editedCommit.commit.author.date" label="Data"></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">Cancelar</v-btn>
                <v-btn color="blue darken-1" text @click="save">Salvar</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-icon small class="mr-2" @click="editItem(item)">mdi-pencil</v-icon>
          <v-icon small @click="deleteItem(item)">mdi-delete</v-icon>
        </template>
        <template v-slot:no-data>
          <h4>Carregue algum repositório</h4>
        </template>
      </v-data-table>
    </v-card>
    <v-btn
    class="mt-5 mr-3"
      rounded
      color="primary"
      dark
      @click="request"
    >{{carregando ? 'Carregando...' : 'Carregar'}}</v-btn>
    <v-btn color="primary" class="mt-5" dark rounded @click="dialog = true">Novo item</v-btn>
  </v-container>
</template>

<script>
export default {
  props: {
    repos: {
      type: Object
    }
  },
  data() {
    return {
      dialog: false,
      search: "",
      carregando: false,
      headers: [
        {
          text: "Autor",
          align: "start",
          sortable: false,
          value: "commit.author.name"
        },
        { text: "Mensagem", value: "commit.message" },
        { text: "Data", value: "commit.author.date" },
        { text: "Actions", value: "actions", sortable: false }
      ],
      commits: [],
      editedIndex: -1,
      editedCommit: {
        commit: {
          message: "",
          author: {
            name: "",
            date: ""
          }
        }
      },
      defaultItem: {
        commit: {
          message: "",
          author: {
            name: "",
            date: ""
          }
        }
      }
    }
  },

  watch: {
    dialog(val) {
      val || this.close()
    }
  },
  methods: {
    request() {
      this.carregando = true

      var url = "https://api.github.com/repos/" + this.repos.user + "/" + this.repos.name + "/commits" //Sua URL
      var xhttp = new XMLHttpRequest()
      xhttp.open("GET", url, true)
      const instancia = this
      xhttp.onreadystatechange = function() {
        if (xhttp.readyState == 4 && xhttp.status == 200) {
          instancia.commits = JSON.parse(xhttp.responseText)
          instancia.carregando = false
          instancia.$emit("clique-tabela", instancia.commits[0])
        }
      }

      xhttp.send()
    },

    cliqueLinha(item) {
      this.$emit("clique-tabela", item)
    },

    editItem(item) {
      this.editedIndex = this.commits.indexOf(item)
      this.editedCommit = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      const index = this.commits.indexOf(item)
      confirm("Você tem certeza que deseja excluir?") && this.commits.splice(index, 1)
    },

    close() {
      this.dialog = false
      setTimeout(() => {
        this.editedCommit = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      }, 300)
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.commits[this.editedIndex], this.editedCommit)
      } else {
        this.commits.push(this.editedCommit)
      }
      this.close()
    }
  }
}
</script>
