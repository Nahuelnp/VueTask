<template>
  <v-container grid-list-xs>
    <v-row justify="center">
      <v-col cols="12" md="4" sm="6" lg="4">
        <h3 class="my-2">Nueva Tarea</h3>
        <v-form @submit="newTask">
        <v-text-field
          background-color="white"
          outlined
          label="Titulo"
          v-model="form.title"
          required
        ></v-text-field>
        <v-textarea
          background-color="white"
          outlined
          name="input-7-4"
          label="Descripcion"
          rows="3"
          v-model="form.text"
          required
        ></v-textarea>
        <v-btn block color="teal lighten-2" type="submit">
          <v-icon>mdi-plus</v-icon>
        </v-btn>
        </v-form>
        
      </v-col>

      <v-col cols="12" md="4" sm="6" lg="4">
        <h3 class="my-2">Vista Previa</h3>
        <v-card
          class="mx-auto"
          max-width="344"
          color="yellow lighten-2"
          elevation="5"
        >
          <v-card-title>
            {{ form.title }}
          </v-card-title>

          <v-card-text>
            {{ form.text }}
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-dialog v-model="dialog" max-width="300">
      <v-card color="teal lighten-5">
        <v-card-title class="headline">
          Tarea Creada
        </v-card-title>
        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="black" text @click="changeDialog">
            Ok
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      dialog: false,
      form: {
        title: " ",
        descriptio: " ",
        done: false,
        deleted: false
      }
    };
  },
  methods: {
    newTask(evt) {
      evt.preventDefault();
      let formulario = new FormData();
      formulario.append("title", this.form.title);
      formulario.append("text", this.form.text);
      formulario.append("done", this.form.done);
      formulario.append("deleted", this.form.deleted);

      const path = "https://nahuelapitask.herokuapp.com/api/tasks/";
      axios
        .post(path, formulario)
        .then(response => {
          console.log("Tarea Cargada Exitosamente");
        })
        .catch(error => {
          console.log(error);
        });

      this.form.title = " ";
      this.form.text = " ";
      this.dialog = true;
    },
    changeDialog() {
      this.dialog = false;
    }
  }
};
</script>