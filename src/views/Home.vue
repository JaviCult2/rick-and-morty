<template>

  <div class="container">

    <div class="text-center">
      <v-container>
        <v-row justify="center">
          <v-col cols="8">
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

      <div class="col-12 col-sm-4 my-5" v-for="(item,index) of characters" :key="index">

        <v-card :loading="loading">

          <template slot="progress">
            <v-progress-linear
                color="deep-purple"
                height="10"
                indeterminate
            ></v-progress-linear>
          </template>

          <v-img :src="item.image"/>

          <v-card-title>{{ item.name }}</v-card-title>

          <v-card-text>
            <v-chip-group
                v-model="selection"
                active-class=""
                column
            >

              <v-chip :class="item.gender === 'Male' ? 'c-blue' : ( item.gender === 'Female' ? 'c-pink':'c-purple')"
                      class="text-white">
                {{ item.gender }}
              </v-chip>

              <v-chip :class="item.gender === 'Human' ? 'c1' : 'c2'" class="text-white">
                {{ item.species }}
              </v-chip>

              <v-chip :class="item.status === 'Alive' ? 'c-green' : (item.status === 'Dead') ? 'c-red' : 'c-purple'"
                      class="text-white">
                {{ item.status }}
              </v-chip>

            </v-chip-group>

          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>
            <character-detail :url="item.url" :url-location="item.location"/>
          </v-card-actions>
        </v-card>

      </div>

    </div>

    <div class="text-center">
      <v-container>
        <v-row justify="center">
          <v-col cols="8">
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


import AddressModal from "@/components/AddressModal";
import axios from 'axios';
import CharacterDetail from "@/components/CharacterDetail";

export default {
  name: "Home",
  components: {CharacterDetail, AddressModal},
  async mounted() {
    await this.getData();
  },
  data: () => ({

    loading: false,
    selection: 1,
    page: 1,

    length: 0,

    characters: [],

  }),
  methods:
      {
        reserve() {
          this.loading = true

          setTimeout(() => (this.loading = false), 2000)
        },

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
        page: function dialog(value) {
          this.getData()
        },
      }
}
</script>

<style scoped>

.container {
  background-color: darkslategray;
}

.c-pink {
  background: #EE587B !important;
}

.c-blue {
  background: #4D4D81 !important;
}

.c-green {
  background-color: #1ABC9C !important;
}

.c-red {
  background-color: #FC210D !important;
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