<template>
  <v-card variant="flat">
    <v-layout>
      <v-app-bar
        color="primary"
      >
        <template v-slot:prepend>
          <v-app-bar-nav-icon></v-app-bar-nav-icon>
        </template>

        <v-app-bar-title>
          <strong>UN1D3V</strong> Team Manager
        </v-app-bar-title>
      </v-app-bar>

      <v-main>
        <v-container fluid>
          <v-row>
            <v-col
              cols="12"
            >
              
              <create-player></create-player>

            </v-col>
          </v-row>
        </v-container>
      </v-main>
    </v-layout>
  </v-card>
</template>

<script>

import CreatePlayer from './components/CreatePlayer.vue';

export default {
  components: {
    CreatePlayer
  }
}

</script>
