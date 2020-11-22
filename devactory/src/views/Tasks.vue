<template>
  <div>
    <v-container grid-list-xs>
      <v-row>
        <v-col cols="12" lg="3" md="3">
          <v-row >
            <v-col cols ="12" lg="12" md="12">
            <v-card class="flex mx-auto text-center" max-width="344" >
              <v-card-title >
                Nueva Tarea
              </v-card-title>
              <v-card-text>

        <v-btn  block color="teal lighten-2"  :to="{ name: 'Add' }">
          <v-icon>mdi-plus</v-icon>
        </v-btn>
              </v-card-text>
            </v-card>
            </v-col>
          </v-row>
        </v-col>

        <v-spacer></v-spacer>

        <v-col cols="12" lg="9" md="9">
          <v-row>
            <v-col
              cols="12"
              md="4"
              sm="6"
              lg="4"
              v-for="item in items"
              :key="item.id"
              v-if="!item.deleted"
            >
              <v-card
                class="mx-auto"
                max-width="344"
                color="yellow lighten-2"
                elevation="5"
              >
                <v-card-title class="text" style="word-break: inherit;">
                  
                  {{ item.title }}
                 
                </v-card-title>
                <v-card-text>
                  {{ item.text }}
                </v-card-text>
                <v-card-actions>
                  <v-btn icon @click="doneTask(item)" small>
                    <v-icon color="blue">mdi-check</v-icon></v-btn
                  >
                  <v-btn icon @click="editTask(item)" small>
                    <v-icon color="green">mdi-pencil-outline</v-icon>
                  </v-btn>
                  <v-btn icon @click="deleteTask(item)" v-if="!item.done" small
                    ><v-icon color="red">mdi-delete</v-icon></v-btn
                  >
                </v-card-actions>
                <v-overlay :absolute="true" :value="item.done">
                  HECHO
                  <v-btn icon @click="deleteTask(item)" color="red lighten-1"
                    ><v-icon>mdi-delete</v-icon></v-btn
                  ></v-overlay
                >
              </v-card>
            </v-col>
          </v-row>
        </v-col>
      </v-row>

      <v-dialog v-model="dialog" max-width="500px">
        <template v-if="editar">
          <div>
            <v-card color="teal lighten-5">
              <v-col cols="12">
                <v-form @submit="onSubmit">
                  <v-text-field
                    background-color="white"
                    outlined
                    label="Title"
                    v-model="form.title"
                  ></v-text-field>
                  <v-textarea
                    background-color="white"
                    outlined
                    name="input-7-4"
                    label="Description"
                    rows="3"
                    v-model="form.text"
                  ></v-textarea>
                  <v-btn color="teal" type="submit">
                    Actualizar
                  </v-btn>
                </v-form>
              </v-col>
            </v-card>
          </div>
        </template>
        <template v-else>
          <div>
            <v-dialog v-model="dialog" persistent max-width="300">
              <v-card color="teal lighten-5">
                <v-card-title class="headline">
                  Tarea no terminada, desea eliminarla de todos modos?
                </v-card-title>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="black" text @click="dialog = false">
                    Cancelar
                  </v-btn>
                  <v-btn color="black" text @click="confirmDelete(idNew)">
                    Confirmar
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </div>
        </template>
      </v-dialog>
    </v-container>
  </div>
</template>
<script>
import Vue from "vue";
import axios from "axios";
export default {
  data() {
    return {
      dialog: false,
      idNew: null,
      editar: true,
      items: [],
      form: {
        title: "",
        text: "",
        done: false,
        deleted: false
      }
    };
  },
  methods: {
    doneTask(tarea) {
      this.form.done = true;
      this.form.text = tarea.text;
      this.form.title = tarea.title;
      this.form.deleted = tarea.deleted;
      this.items[tarea.id - 1].done = true;
      const path = `https://nahuelapitask.herokuapp.com/api/tasks/${tarea.id}/`;
      axios
        .put(path, this.form)
        .then(response => {
          console.log("tarea actualizada");
        })
        .catch(error => {
          console.log(error);
        });
    },
    editTask(tarea) {
      this.form.title = tarea.title;
      this.form.text = tarea.text;
      this.dialog = true;
      this.editar = true;
      this.idNew = tarea.id;
    },
    onSubmit(evt) {
      evt.preventDefault();
      const path = `https://nahuelapitask.herokuapp.com/api/tasks/${this.idNew}/`;
      axios
        .put(path, this.form)
        .then(response => {
          console.log("tarea editada");
        })
        .catch(error => {
          console.log(error);
        });
      this.items[this.idNew - 1].title = this.form.title;
      this.items[this.idNew - 1].text = this.form.text;
      this.dialog = false;
      this.idNew = null;
    },
    deleteTask(tarea) {
      this.form.deleted = true;
      this.form.title = tarea.title;
      this.form.text = tarea.text;
      this.form.done = tarea.done;
      this.idNew = tarea.id;
      if (tarea.done) {
        this.items[tarea.id - 1].deleted = true;
        this.confirmDelete(this.idNew);
      } else {
        this.dialog = true;
        this.editar = false;
      }
    },
    confirmDelete(id) {
      this.dialog = false;
      this.items[id - 1].deleted = true;
      const path = `https://nahuelapitask.herokuapp.com/api/tasks/${id}/`;
      axios
        .put(path, this.form)
        .then(response => {
          console.log("eliminado");
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  created() {
    axios
      .get("https://nahuelapitask.herokuapp.com/api/tasks/")
      .then(response => (this.items = response.data))
      .catch(error => {
        console.log(error);
      });
  }
};
</script>

