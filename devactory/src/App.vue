<template>
  <v-app>
    <v-container grid-list-xs>
      <v-card color="teal lighten-5" class="flex">
        <v-bottom-navigation
          background-color="teal"
          v-model="value"
          color="white"
        >
          <v-btn :to="{ name: 'Tasks' }" exact>
            <span>Tareas</span>

            <v-icon>mdi-view-list</v-icon>
          </v-btn>

          <v-btn :to="{ name: 'Add' }">
            <span>Nueva Tarea</span>

            <v-icon>mdi-plus-box</v-icon>
          </v-btn>
        </v-bottom-navigation>
        <v-main>
          <router-view></router-view>
        </v-main>
        <v-footer dark padless>
          <v-card class="flex" flat tile>
            <v-card-title class="teal py-2">

            </v-card-title>
            <v-card-text class="py-2 white--text text-center">
              {{ new Date().getFullYear() }} —
              <strong>Pedroso Nahuel Nicolas</strong>
            </v-card-text>
          </v-card>
        </v-footer>
      </v-card>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      value: 1
    };
  },
  computed: {}
};
</script>
