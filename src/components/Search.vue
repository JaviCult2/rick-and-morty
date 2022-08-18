<template>

  <section class="container">

    <div class="row">

      <div class="col-12">

        <v-form v-model="valid" ref="form">

          <div class="row col-12 mx-auto">

            <div class="col-12 col-sm-10">
              <v-text-field
                  v-model="name"
                  :rules="rules.name"
                  label="Write the name to search"
                  clearable
                  solo
              ></v-text-field>
            </div>

            <div class="col-12 col-sm-2">

              <v-btn
                  rounded
                  color="#85929E"
                  dark
                  block
                  v-on:click="searchCharacter"
              >
                Search
              </v-btn>

            </div>

          </div>

        </v-form>

      </div>

      <div class="col-12">

        <div v-if="characters.length !== 0">
          <p class="text-white">{{ switchAux ? 'Show' : 'Hide' }}</p>
          <v-switch v-model="switchAux"/>
        </div>

        <v-card class="mx-auto" v-if="characters.length !== 0" :class="!switchAux ? 'd-none':''">

          <v-list three-line>

            <div class="row">

              <div class="col-12 col-sm-4" v-for="(item, index) in characters">

                <v-list-item :key="index">

                  <v-list-item-avatar>
                    <v-img :src="item.image"></v-img>
                  </v-list-item-avatar>

                  <v-list-item-content>
                    <v-list-item-title v-html="item.name"/>
                  </v-list-item-content>

                  <v-list-item-action>
                    <character-detail version="v2" :url="item.url" :url-location="item.location"/>
                  </v-list-item-action>

                </v-list-item>

              </div>

            </div>


          </v-list>

        </v-card>

      </div>

    </div>


  </section>

</template>

<script>
import axios from "axios";
import CharacterDetail from "@/components/CharacterDetail";

export default {
  name: "Search",
  components: {CharacterDetail},
  data: () => ({
    valid: false,
    switchAux: true,

    name: null,


    characters: [
      /*{
        image: 'https://media.sproutsocial.com/uploads/2017/02/10x-featured-social-media-image-size.png',
        name: 'Prueba'
      }*/
    ],

    rules:
        {
          name: [v => !!v || 'The name is required']
        }
  }),
  methods:
      {
        async searchCharacter() {

          if (this.$refs.form.validate()) {

            this.characters = []

            await axios.get(`https://rickandmortyapi.com/api/character/?name=${this.name}`).then((response) => {

              response.data.results.forEach(item => {
                let character =
                    {
                      image: item.image,
                      name: item.name,
                      url: item.url,
                      location: item.location.url
                    }

                this.characters.push(character)
              })

            });

          } else {
            console.log("No pasas")
            console.log("Proximamente pondré un alert")
          }
        }
      }
}
</script>

<style scoped>

.c-blue {
  background: #4D4D81 !important;
}

</style>