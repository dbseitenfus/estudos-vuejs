<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      :clipped="$vuetify.breakpoint.lgAndUp"
      app
    >
      <v-list dense>
        <template v-for="(item, i) in items">
          <v-row
            v-if="item.heading"
            :key="item.heading"
            align="center"
          >
            <v-col cols="6">
              <v-subheader v-if="item.heading">
                {{ item.heading }}
              </v-subheader>
            </v-col>
            <v-col
              cols="6"
              class="text-center"
            >
              <a
                href="#!"
                class="body-2 black--text"
              >EDIT</a>
            </v-col>
          </v-row>
          <v-list-group
            v-else-if="item.children"
            :key="item.text"
            v-model="item.model"
            :prepend-icon="item.model ? item.icon : item['icon-alt']"
            append-icon=""
          >
            <template v-slot:activator>
              <v-list-item-content>
                <v-list-item-title>
                  {{ item.text }}
                </v-list-item-title>
              </v-list-item-content>
            </template>
            <v-list-item
              v-for="(child, i) in item.children"
              :key="i"
              link
            >
              <v-list-item-action v-if="child.icon">
                <v-icon>{{ child.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>
                  {{ child.text }}
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-group>
          <v-list-item
            v-else
            :key="item.text"
            link
            @click="cliqueItemMenu(i)"
          >
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>
                {{ item.text }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      app
      color="blue darken-3"
      dark
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title
        style="width: 300px"
        class="ml-0 pl-4"
      >
        <span class="hidden-sm-and-down">Estudos Vue e Vuetify</span>
      </v-toolbar-title>
      <v-text-field
        flat
        solo-inverted
        hide-details
        prepend-inner-icon="mdi-magnify"
        label="Search"
        class="hidden-sm-and-down"
      />
      <v-spacer />
      <v-btn icon>
        <v-icon>mdi-apps</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>mdi-bell</v-icon>
      </v-btn>
      <v-btn
        icon
        large
      >
        <v-avatar
          size="32px"
          item
        >
          <v-img
            src="https://cdn.vuetifyjs.com/images/logos/logo.svg"
            alt="Vuetify"
          /></v-avatar>
      </v-btn>
    </v-app-bar>
    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
        <v-row
          align="center"
          justify="center">

          <About v-show="indexMenu==0"/>
          <CommitViewer v-show="indexMenu==1"/>
          <RandowMealGenerator v-show="indexMenu==2" @registerLog="registerLog($event)"/>
          <Dashboard v-if="indexMenu==3" :dashboard="dashboard"/>
          <Administrators v-if="indexMenu==4"/>
          <Chart v-if="indexMenu==5"/>
        </v-row>
      </v-container>
    </v-content>
    
    <v-dialog
      v-model="dialog"
      width="800px"
    >
      <v-card>
        <v-card-title class="grey darken-2">
          Create contact
        </v-card-title>
        <v-container>
          <v-row class="mx-2">
            <v-col
              class="align-center justify-space-between"
              cols="12"
            >
              <v-row
                align="center"
                class="mr-0"
              >
                <v-avatar
                  size="40px"
                  class="mx-3"
                >
                  <img
                    src="//ssl.gstatic.com/s2/oz/images/sge/grey_silhouette.png"
                    alt=""
                  >
                </v-avatar>
                <v-text-field
                  placeholder="Name"
                />
              </v-row>
            </v-col>
            <v-col cols="6">
              <v-text-field
                prepend-icon="mdi-account-card-details-outline"
                placeholder="Company"
              />
            </v-col>
            <v-col cols="6">
              <v-text-field
                placeholder="Job title"
              />
            </v-col>
            <v-col cols="12">
              <v-text-field
                prepend-icon="mdi-mail"
                placeholder="Email"
              />
            </v-col>
            <v-col cols="12">
              <v-text-field
                type="tel"
                prepend-icon="mdi-phone"
                placeholder="(000) 000 - 0000"
              />
            </v-col>
            <v-col cols="12">
              <v-text-field
                prepend-icon="mdi-text"
                placeholder="Notes"
              />
            </v-col>
          </v-row>
        </v-container>
        <v-card-actions>
          <v-btn
            text
            color="primary"
          >More</v-btn>
          <v-spacer />
          <v-btn
            text
            color="primary"
            @click="dialog = false"
          >Cancel</v-btn>
          <v-btn
            text
            @click="dialog = false"
          >Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
import CommitViewer from './pages/CommitViewer'
import RandowMealGenerator from './pages/RandowMealGenerator'
import Dashboard from './pages/Dashboard'
import Chart from './pages/Chart'
import About from './pages/About'
import Administrators from './pages/Administrators'

  export default {
    components:{
      CommitViewer, RandowMealGenerator, Dashboard, Chart, About, Administrators
    },
    props: {
      source: String,
    },
    data: () => ({
      dashboard: {
        views:{
          page0: 1,
          page1: 0,
          page2: 0,
          page3: 0,
          meal: []
        },
        likes:{
          meal: []
        },
        ratings:{
          meal: [{name: 'Vegetarian', 'Pontuação': 0, 'Qt. de avaliações': 0},
          {name: 'Beef', 'Pontuação': 0, 'Qt. de avaliações': 0},
          {name: 'Pork', 'Pontuação': 0, 'Qt. de avaliações': 0},
          {name: 'Dessert', 'Pontuação': 0, 'Qt. de avaliações': 0},
          {name: 'Pasta', 'Pontuação': 0, 'Qt. de avaliações': 0}]
        }
      },
      indexMenu: 0,
      dialog: false,
      drawer: null,
      items: [
        { icon: 'mdi-information-outline', text: 'About' },
        { icon: 'mdi-github', text: 'Commit Viewer' },
        { icon: 'mdi-food', text: 'Random Meal Generator' },
        { icon: 'mdi-monitor-dashboard', text: 'Dashboard' },
        { icon: 'mdi-account-key', text: 'Administrators' },
        { icon: '', text: 'Mais gráficos' }
      ],
    }),
    methods:{
      cliqueItemMenu(index){
        this.dashboard.views[`page${index}`]++;
        this.indexMenu = index;
        if(index === 3){
          this.drawer = false;
        }
      },
      registerLog(event){
        let encontrei = false;
        if(event.category === "like-meal"){
          this.dashboard.likes.meal.forEach(log => {
            if(log.name === event.value.strCategory){
              log.value++;
              encontrei = true;
            }
          });
          if(!encontrei){
            this.dashboard.likes.meal.push({value: 1, name: event.value.strCategory});
          }
        }else if(event.category === "rating-meal"){
           this.dashboard.ratings.meal.forEach(log => {
            if(log.name === event.value.strCategory){
              log['Pontuação'] += event.rating;
              log['Qt. de avaliações']++;
              encontrei = true;
            }
          });
          if(!encontrei){
            this.dashboard.ratings.meal.push({name: event.value.strCategory, 'Pontuação': event.rating, 'Qt. de avaliações': 1});
          }

        }
      }
    }
  }
</script>