<template>
  <v-component class="pa-5 ml-5 mr-5" style="height: 100%; width: 100%">
    <v-dialog v-model="dialog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <div class="ma-5">
            <span class="headline">Administrador</span>
            <v-btn color="blue" class="white--text mr-5" align="right" absolute right>
              <v-icon dark>mdi-account-plus</v-icon>
            </v-btn>
          </div>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col>
                <v-text-field
                  v-if="index >= 0"
                  v-model="items[index].title"
                  label="Nome*"
                  required
                >{{items[index].title}}</v-text-field>
                <v-text-field
                  v-else
                  v-model="novoAdm.title"
                  label="Nome*"
                  required
                >{{novoAdm.title}}</v-text-field>
              </v-col>
              <v-col>
                <v-text-field
                  v-if="index >= 0"
                  v-model="items[index].celular"
                  label="Celular*"
                  hint="example of helper text only on focus"
                >{{items[index].celular}}</v-text-field>
                <v-text-field
                  v-else
                  v-model="novoAdm.celular"
                  label="Celular*"
                  hint="example of helper text only on focus"
                >{{novoAdm.celular}}</v-text-field>
              </v-col>

              <v-col cols="12">
                <v-expansion-panels accordion>
                  <v-expansion-panel>
                    <v-expansion-panel-header>Permissões</v-expansion-panel-header>
                    <v-expansion-panel-content v-if="index >= 0">
                      <v-switch
                        v-for="(item,i) in items[index].permissoes"
                        :key="i"
                        v-model="item.possui"
                        :label="item.nome"
                      ></v-switch>
                    </v-expansion-panel-content>
                    <v-expansion-panel-content v-else>
                      <v-switch
                        v-for="(item,i) in novoAdm.permissoes"
                        :key="i"
                        v-model="item.possui"
                        :label="item.nome"
                      ></v-switch>
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                </v-expansion-panels>
              </v-col>
            </v-row>
          </v-container>
          <small>*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialog = false">Fechar</v-btn>
          <v-btn color="blue darken-1" text @click="salvar">Salvar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-list three-line>
      <template v-for="(item, index) in items">
        <v-subheader v-if="item.header" :key="item.header" v-text="item.header"></v-subheader>

        <v-divider v-else-if="item.divider" :key="index" :inset="item.inset"></v-divider>

        <v-list-item v-else :key="item.title" @click="cliqueAdm(index)">
          <v-list-item-avatar>
            <v-img :src="item.avatar"></v-img>
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title v-html="item.title"></v-list-item-title>
            <v-list-item-subtitle v-html="item.subtitle"></v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </template>
    </v-list>
    <v-btn color="blue" class="mb-5 white--text" fab right bottom fixed @click="addAdm">
      <v-icon dark>mdi-account-plus</v-icon>
    </v-btn>
  </v-component>
</template>

<script>
export default {
  data() {
    return {
      index: -1,
      dialog: false,
      items: [
        { header: "Administradores" },
        {
          avatar: "https://cdn.vuetifyjs.com/images/lists/1.jpg",
          title: "Daniel Seitenfus",
          subtitle: "Proprietário",
          celular: "(51) 99124-2937",
          permissoes: [
            { nome: "Marcar horários", possui: true },
            { nome: "Financeiro", possui: true },
            { nome: "Enviar lembrete", possui: true },
            { nome: "Adicionar administrador", possui: true },
            { nome: "Modificar informações de cadastro", possui: true }
          ]
        },
        { divider: true, inset: true },
        {
          avatar: "https://cdn.vuetifyjs.com/images/lists/2.jpg",
          title: "Lucas Real",
          subtitle: "Administrador",
          celular: "(51) 99124-2937",
          permissoes: [
            { nome: "Marcar horários", possui: true },
            { nome: "Financeiro", possui: false },
            { nome: "Enviar lembrete", possui: false },
            { nome: "Adicionar administrador", possui: false },
            { nome: "Modificar informações de cadastro", possui: true }
          ]
        },
        { divider: true, inset: true },
        {
          avatar: "https://cdn.vuetifyjs.com/images/lists/3.jpg",
          title: "Marcelo Hoss",
          subtitle: "Administrador",
          celular: "(51) 99124-2937",
          permissoes: [
            { nome: "Marcar horários", possui: true },
            { nome: "Financeiro", possui: false },
            { nome: "Enviar lembrete", possui: true },
            { nome: "Adicionar administrador", possui: true },
            { nome: "Modificar informações de cadastro", possui: true }
          ]
        },
        { divider: true, inset: true },
        {
          avatar: "https://cdn.vuetifyjs.com/images/lists/4.jpg",
          title: "Fernando",
          subtitle: "Administrador",
          celular: "(51) 99124-2937",
          permissoes: [
            { nome: "Marcar horários", possui: true },
            { nome: "Financeiro", possui: false },
            { nome: "Enviar lembrete", possui: true },
            { nome: "Adicionar administrador", possui: true },
            { nome: "Modificar informações de cadastro", possui: true }
          ]
        },
        { divider: true, inset: true },
        {
          avatar: "https://cdn.vuetifyjs.com/images/lists/5.jpg",
          title: "Pâmela",
          subtitle: "Administrador",
          celular: "(51) 99124-2937",
          permissoes: [
            { nome: "Marcar horários", possui: true },
            { nome: "Financeiro", possui: false },
            { nome: "Enviar lembrete", possui: true },
            { nome: "Adicionar administrador", possui: true },
            { nome: "Modificar informações de cadastro", possui: true }
          ]
        }
      ],
      novoAdm: {
        avatar: "https://cdn.vuetifyjs.com/images/lists/1.jpg",
        title: "",
        subtitle: "Administrador",
        celular: "",
        permissoes: [
          { nome: "Marcar horários", possui: true },
          { nome: "Financeiro", possui: true },
          { nome: "Enviar lembrete", possui: true },
          { nome: "Adicionar administrador", possui: true },
          { nome: "Modificar informações de cadastro", possui: true }
        ]
      }
    }
  },
  methods: {
    cliqueAdm(index) {
      this.index = index
      this.dialog = true
    },
    addAdm() {
      this.index = -1
      this.dialog = true
    },
    salvar() {
      this.dialog = false
      if (this.index == -1) {
        this.items.push({ divider: true, inset: true })
        this.items.push(this.novoAdm)
      }
    }
  }
}
</script>
<style>
</style>