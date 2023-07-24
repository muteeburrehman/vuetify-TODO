<template>
  <v-app id="inspire">
    <v-navigation-drawer
        v-model="drawer"
        app
    >
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="title">
            Vuetify Todo
          </v-list-item-title>
          <v-list-item-subtitle>
            Best Todo Ever!
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list
          dense
          nav>
        <v-list-item
            v-for="item in items"
            :key="item.title"
            :to="item.to"
            link
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>
              {{ item.title }}
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar

        color="primary"
        app
        dark
        src="forest.jpg"
        prominent
    >
      <template v-slot:img="{props}">
        <v-img
            v-bind="props"
            gradient="to top right,rgba(19,84,122,.5),
          rgba(128,208,199,.8)"></v-img>
      </template>

      <v-appbar-nav-icon @click="drawer=!drawer">
        <v-btn icon>
          <v-icon>mdi-menu</v-icon>
        </v-btn>
      </v-appbar-nav-icon>
      <v-app-bar-title>
        Vuetify Todo
      </v-app-bar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <!-- Your content goes here -->
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>
import {
  VApp,
  VNavigationDrawer,
  VAppBar,
  VMain,
  VList,
  VListItem,
  VListItemAction,
  VListItemContent,
  VListItemTitle,
  VDivider,
  VIcon,
  VAppbarNavIcon,
  VAppbarTitle,
  VBtn,
  VSpacer
} from 'vuetify/lib';

export default {
  components: {
    VApp,
    VNavigationDrawer,
    VAppBar,
    VMain,
    VList,
    VListItem,
    VListItemAction,
    VListItemContent,
    VListItemTitle,
    VDivider,
    VIcon,
    VAppbarNavIcon,
    VAppbarTitle,
    VBtn,
    VSpacer
  },
  data: () => ({
    drawer: null,
    items: [
      {
        title: 'Todo',
        icon: 'mdi-format-list-checks',
        to: '/'
      },
      {
        title: 'About',
        icon: 'mdi-help-box',
        to: '/about'
      },
    ],
  })
}
</script>

<style>
</style>