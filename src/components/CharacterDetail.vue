<template>

  <div class="text-center">

    <v-dialog v-model="dialog" max-width="600" persistent>

      <template v-slot:activator="{ on, attrs }">

        <v-btn
            color="#34495E"
            rounded
            dark
            v-bind="attrs"
            v-on="on"
            x-small
            v-on:click="getData"
        >
          <v-icon dark x-small>mdi-eye</v-icon>
          <div v-if="version==='v1'">
            Show more
          </div>
        </v-btn>
      </template>

      <v-card class="c-blue">

        <v-card-title>
          <h2 class="text-white">{{ this.character.name }}</h2>
        </v-card-title>

        <v-card-text>

          <div class="text-center my-2">
            <img :src="this.character.image" :alt="this.character.name" class="w-100">
          </div>

          <v-stepper v-model="e6" vertical>

            <v-stepper-step step="1" @click="e6 = 1" color="#4D4D81">
              Character
            </v-stepper-step>

            <v-stepper-content step="1">
              <v-card color="" class="p-3">
                <p class="text-black">Name: <br> <b class="c-red">{{ character.name }}</b></p>
                <p class="text-black">Gender: <br> <b class="c-red">{{ character.gender }}</b></p>
                <p class="text-black">Specie: <br> <b class="c-red">{{ character.species }}</b></p>
                <p class="text-black">Status: <br> <b class="c-red">{{ character.status }}</b></p>
              </v-card>
            </v-stepper-content>

            <v-stepper-step step="2" @click="e6 = 2" color="#4D4D81">
              Location
            </v-stepper-step>

            <v-stepper-content step="2">
              <v-card class="p-3">
                <p class="text-black">Name: <br> <b class="c-red">{{ location.name }}</b></p>
                <p class="text-black">Dimension: <br> <b class="c-red">{{ location.dimension }}</b></p>
                <p class="text-black">Type: <br> <b class="c-red">{{ location.type }}</b></p>
              </v-card>

            </v-stepper-content>

          </v-stepper>

        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
              color="#fff"
              text
              @click="dialog = false"
          >
            Close
          </v-btn>
        </v-card-actions>
      </v-card>

    </v-dialog>

  </div>

</template>

<script>
import axios from "axios";

export default {
  name: "CharacterDetail",
  props:
      {
        version: {value: String, default: 'v1'},
        url: '',
        urlLocation: ''
      },
  data: () => ({
    dialog: false,
    e6: 1,
    character:
        {
          name: null,
          status: null,
          species: null,
          image: null,
          gender: null,
          origin: null,
          url: null,
        },
    location:
        {
          name: null,
          dimension: null,
          type: null
        }
  }),
  methods:
      {
        async getData() {
          await this.getCharacter()
          await this.getLocation()
        },
        async getCharacter() {
          await axios.get(this.url).then((response) => {

            let data = response.data

            let character =
                {
                  name: data.name,
                  status: data.status,
                  species: data.species,
                  image: data.image,
                  gender: data.gender,
                  origin: data.origin.name,
                  url: data.url
                }

            this.character = character;

          });
        },
        async getLocation() {
          await axios.get(this.urlLocation).then((response) => {

            let data = response.data

            let location = {
              name: data.name,
              dimension: data.dimension,
              type: data.type
            }

            this.location = location

          });
        }
      }
}
</script>

<style scoped>

.c-blue {
  background-color: #4D4D81 !important;
}

.c-red {
  color: #C70039;
}

@media (min-width: 575px) {
  p br {
    display: none;
  }
}

</style>