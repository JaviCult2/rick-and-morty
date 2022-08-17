<template>

  <div class="container-fluid bg-container">

    <div class="text-center">
      <v-container>
        <v-row justify="center">
          <v-col cols="10">
            <v-container class="max-width">
              <v-pagination
                  v-model="page"
                  class="my-4"
                  :length="length"
              ></v-pagination>
            </v-container>
          </v-col>
        </v-row>
      </v-container>
    </div>

    <div class="row">

      <div class="col-12 col-sm-4 my-5 d-flex justify-content-center align-items-stretch"
           v-for="(item,index) of characters" :key="index">

        <v-card class="card-round" min-width="200" max-width="350">

          <v-img :src="item.image" class="p-image"/>

          <v-card-title>{{ item.name }}</v-card-title>

          <v-card-text>

            <v-chip-group column>

              <v-chip
                  class="col-12 col-sm-8 col-lg-5 mx-sm-auto mx-lg-0 text-white"
                  :class="item.gender === 'Male' ? 'c-blue' : (item.gender === 'Female' ? 'c-pink':'c-purple')">
                <b>Gender: </b> {{ item.gender }}
              </v-chip>

              <v-chip
                  class="col-12 col-sm-8 col-lg-5 mx-sm-auto mx-lg-0 text-white"
                  :class="item.gender === 'Human' ? 'c1' : 'c2'">
                <b>Specie: </b> {{ item.species }}
              </v-chip>

              <v-chip
                  class="col-12 col-sm-8 col-lg-5 mx-sm-auto mx-lg-0 text-white"
                  :class="item.status === 'Alive' ? 'c-green' : (item.status === 'Dead') ? 'c-red' : 'c-purple'">
                <b>Status: </b> {{ item.status }}
              </v-chip>

            </v-chip-group>

          </v-card-text>

          <v-card-actions>
            <v-spacer/>
            <character-detail :url="item.url" :url-location="item.location"/>
          </v-card-actions>

        </v-card>

      </div>

    </div>

    <div class="text-center">
      <v-container>
        <v-row justify="center">
          <v-col cols="10">
            <v-container class="max-width">
              <v-pagination
                  v-model="page"
                  class="my-4"
                  :length="length"
              ></v-pagination>
            </v-container>
          </v-col>
        </v-row>
      </v-container>
    </div>

  </div>

</template>

<script>

import axios from 'axios';
import CharacterDetail from "@/components/CharacterDetail";

export default {
  name: "Home",
  components: {CharacterDetail},
  async mounted() {
    await this.getData();
  },
  data: () => ({

    page: 1,
    length: 0,

    characters: []
  }),
  methods:
      {
        async getData() {

          this.characters = []

          await axios.get(`https://rickandmortyapi.com/api/character/?page=${this.page}`).then((response) => {

            this.length = response.data.info.pages

            let data = response.data.results

            data.forEach((item) => {

              let character =
                  {
                    name: item.name,
                    status: item.status,
                    species: item.species,
                    image: item.image,
                    gender: item.gender,
                    origin: item.origin.name,
                    url: item.url,
                    location: item.location.url
                  }

              this.characters.push(character);
            });

          });

        }

      },
  watch:
      {
        page: function dialog() {
          this.getData()
        }
      }
}
</script>

<style scoped>

.bg-container {
  background: linear-gradient(to right, #17202A, #1C2833, #212F3D, #283747, #000);
}

.card-round {
  border-radius: 1rem
}

.p-image {
  max-height: 300px;
}

/*Colors*/

.c-pink {
  background: #BE74B5 !important;
}

.c-blue {
  background: #4D4D81 !important;
}

.c-green {
  background-color: #1ABC9C !important;
}

.c-red {
  background-color: #C70039 !important;
}

.c-purple {
  background-color: #450A3B !important;
}

.c1 {
  background-color: #900C3F !important;
}

.c2 {
  background-color: #C70039 !important;
}


</style>