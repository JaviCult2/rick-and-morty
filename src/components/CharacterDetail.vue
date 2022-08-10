<template>
  <div class="text-center">
    <v-dialog v-model="dialog" max-width="800">
      <template v-slot:activator="{ on, attrs }">
        <v-btn
            color="#ff0033"
            rounded
            dark
            v-bind="attrs"
            v-on="on"
            x-small
            v-on:click="getData"
        >
          <v-icon dark x-small>
            mdi-eye
          </v-icon>
          Show more
        </v-btn>
      </template>

      <v-card>
        <v-card-title class="c-green">
          <h2 class="text-white">{{ this.character.name }}</h2>
        </v-card-title>

        <v-card-text>

          <div class="text-center mt-4">
            <img :src="this.character.image" :alt="this.character.name">
          </div>

          <v-stepper v-model="e6" vertical>

            <v-stepper-step step="1" @click="e6 = 1" color="#263B2B">
              Character
            </v-stepper-step>

            <v-stepper-content step="1">
              <v-card color="#263B2B" class="p-3">
                <p class="text-white">Name: <b>{{ character.name }}</b></p>
                <p class="text-white">Gender: <b>{{ character.gender }}</b></p>
                <p class="text-white">Specie: <b>{{ character.species }}</b></p>
                <p class="text-white">Status: <b>{{ character.status }}</b></p>
              </v-card>
            </v-stepper-content>

            <v-stepper-step step="2" @click="e6 = 2" color="#263B2B">
              Location
            </v-stepper-step>

            <v-stepper-content step="2">
              <v-card color="#263B2B" class="p-3">
                <p class="text-white">Name: <b>{{ location.name }}</b></p>
                <p class="text-white">Dimension: <b>{{ location.dimension }}</b></p>
                <p class="text-white">Type: <b>{{ location.type }}</b></p>
              </v-card>

            </v-stepper-content>

          </v-stepper>

        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
              color="#263B2B"
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

.c-green {
  background-color: #263B2B !important;
}

</style>