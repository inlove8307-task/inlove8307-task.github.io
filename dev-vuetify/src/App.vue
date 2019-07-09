<template>
  <v-app id="app">
    <v-navigation-drawer
      app
      v-model="drawer"
      temporary>
      <v-toolbar
        color="deep-purple darken-1">
        <v-toolbar-title
          class="deep-purple--text text--lighten-5">
          Menu
        </v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn
          flat
          fab
          @click.stop="drawer = !drawer">
          <v-icon
            color="deep-purple lighten-5">
            close
          </v-icon>
        </v-btn>
      </v-toolbar>
      <v-list two-line>
        <v-list-tile to="/">
          <v-list-tile-action>
            <v-icon>event_available</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            TASK
          </v-list-tile-content>
        </v-list-tile>
        <v-divider></v-divider>
        <v-list-tile to="/memo">
          <v-list-tile-action>
            <v-icon>list_alt</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            MEMO
          </v-list-tile-content>
        </v-list-tile>
        <v-divider></v-divider>
        <v-list-tile to="/data">
          <v-list-tile-action>
            <v-icon>inbox</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            DATA
          </v-list-tile-content>
        </v-list-tile>
        <v-divider></v-divider>
      </v-list>
    </v-navigation-drawer>
    <v-navigation-drawer
      app
      v-model="filter"
      temporary
      right>
      <v-toolbar
        color="deep-purple darken-1">
        <v-toolbar-title
          class="deep-purple--text text--lighten-5">
          Filter
        </v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn
          icon
          fab
          @click.stop="filter = !filter">
          <v-icon
            color="deep-purple lighten-5">
            close
          </v-icon>
        </v-btn>
      </v-toolbar>
      <v-container>
        <v-layout column>
          <v-flex>
            <v-menu
              v-model="picker"
              :close-on-content-click="false"
              transition="scale-transition"
              offset-y>
              <template v-slot:activator="{ on }">
                <v-text-field
                  v-model="date"
                  label="MONTH"
                  outline
                  full-width
                  v-on="on">
                </v-text-field>
              </template>
              <v-date-picker
                v-show="picker"
                v-model="date"
                no-title
                type="month"
                locale="ko-kr"
                @input="picker = false">
              </v-date-picker>
            </v-menu>
          </v-flex>
          <v-flex>
            <v-select
              label="TAG"
              outline
              full-width>
            </v-select>
          </v-flex>
          <v-flex>
            <v-text-field
              label="TITLE"
              outline
              full-width>
            </v-text-field>
          </v-flex>
          <v-flex>
            <v-btn
              block
              large
              color="primary">
              Search
            </v-btn>
          </v-flex>
        </v-layout>
      </v-container>
    </v-navigation-drawer>
    <v-toolbar app color="deep-purple darken-1">
      <v-btn
        flat
        fab
        @click.stop="drawer = !drawer"
        color="deep-purple lighten-5">
        <v-icon>menu</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
      <v-toolbar-title
        class="deep-purple--text text--lighten-5">
        title
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        flat
        fab
        @click.stop="filter = !filter"
        color="deep-purple lighten-5">
        <v-icon>
          search
        </v-icon>
      </v-btn>
    </v-toolbar>
    <v-content>
      <v-container fluid>
        <router-view></router-view>
      </v-container>
    </v-content>
  </v-app>
  <!--
  <div id="app">
    <div id="nav">
      <button @click="show = !show"><i class="material-icons">apps</i></button>
      <nav v-show="show" @click="show = false">
        <router-link to="/"><i class="material-icons">event_available</i></router-link>
        <router-link to="/memo"><i class="material-icons">list_alt</i></router-link>
        <router-link to="/data"><i class="material-icons">inbox</i></router-link>
      </nav>
    </div>
    <router-view/>
    <alert></alert>
  </div>
  -->
</template>

<script>
export default {
  name: 'app',
  data(){
    return {
      drawer: false,
      picker: false,
      filter: false
    }
  },
  computed: {
    date(){
      return this.$store.state.date.join('-')
    }
  }
}
</script>

<style lang="scss">
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
}
</style>