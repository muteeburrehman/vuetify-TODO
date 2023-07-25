<template>
  <v-app id="inspire">
    <v-app-bar color="primary" app dark src="forest.jpg" prominent>
      <template v-slot:img="{ props }">
        <v-img v-bind="props" gradient="to top right, rgba(19, 84, 122, .5), rgba(128, 208, 199, .8)"></v-img>
      </template>

      <v-appbar-nav-icon @click="drawer = !drawer">
        <v-btn icon>
          <v-icon>mdi-menu</v-icon>
        </v-btn>
      </v-appbar-nav-icon>

      <!-- Title and Date/Time container -->
      <div class="title-date-time-container">
        <!-- Vuetify Todo title -->
        <span class="v-list-item-title title">Vuetify Todo</span>

        <!-- Date and Time -->
        <span class="current-date-time">{{ currentDateTime }}</span>
      </div>

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

    <!-- Your content goes here -->
    <v-main>
      <router-view></router-view>
    </v-main>

    <v-navigation-drawer v-model="drawer" app>
      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" :to="item.to" link>
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
  VListItemContent,
  VListItemTitle,
  VDivider,
  VIcon,
  VAppbarNavIcon,
  VSpacer,
} from 'vuetify/lib';

export default {
  components: {
    VApp,
    VNavigationDrawer,
    VAppBar,
    VMain,
    VList,
    VListItem,
    VListItemContent,
    VListItemTitle,
    VDivider,
    VIcon,
    VAppbarNavIcon,
    VSpacer,
  },
  data: () => ({
    drawer: null,
    items: [
      {
        title: 'Todo',
        icon: 'mdi-format-list-checks',
        to: '/',
      },
      {
        title: 'About',
        icon: 'mdi-help-box',
        to: '/about',
      },
    ],
    currentDateTime: null,
  }),
  mounted() {
    // Call the function initially to set the date and time immediately
    this.updateDateTime();
    // Call the function every second (1000ms) to keep updating the date and time
    setInterval(this.updateDateTime, 1000);
  },
  methods: {
    updateDateTime() {
      // Get the current date and time
      const now = new Date();
      // Format the date and time as a string
      const formattedDateTime = now.toLocaleString();
      // Update the data property
      this.currentDateTime = formattedDateTime;
    },
  },
};
</script>

<style>
/* Global styles */
body {
  margin: 0;
  font-family: 'Roboto', sans-serif;
}

/* Position the date and time below Vuetify Todo title */
.title-date-time-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-left: 10px;
}

/* Position the date and time slightly down and left */
.current-date-time {
  font-size: 14px;
  color: white;
  text-transform: uppercase;
  font-weight: bold;
  letter-spacing: 1px;
  margin-top: 4px;
}

/* Style for the Vuetify Todo title in the app bar */
.v-app-bar .v-app-bar__title {
  font-weight: bold;
  font-size: 24px;
  letter-spacing: 1px;
  margin-left: 10px;
}

/* Style for the subtitle in the Vuetify Todo drawer */
.v-list-item-subtitle {
  font-size: 14px;
  color: grey;
  letter-spacing: 1px;
}

/* Move the Vuetify Todo title to the left a bit */
.v-list-item-title.title {
  margin-left: 10px;
}

/* Responsive CSS */
/* Add your media queries here if necessary */
</style>
