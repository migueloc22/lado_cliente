<template>
  <div id="app">
    <b-jumbotron bg-variant="info" header="R&M" lead="Personajes">
      <!-- <p>For more information visit website</p> -->
      <!--  <button @click="Fetch" class="btn btn-danger">Consultar</button> -->
    </b-jumbotron>
    <b-container class="bv-example-row">
      <b-card-group columns>
        <!-- <div v-for="carate of  caracterers" v-bind:key="carate.id">
          <b-card v-bind:title='carate.name'
            v-bind:img-src="carate.image"
            img-alt="Image"
            img-top
          >
            <b-card-text>
              This is a wider card with supporting text below as a natural lead-in to additional content.
              This content is a little bit longer.
            </b-card-text>
            <div slot="footer">
              <small class="text-muted">Last updated 3 mins ago</small>
            </div>
          </b-card>
        </div>-->
        <Character
          v-for="character of  caracterers"
          v-bind:key="character.id"
          v-bind:character="character"
        />
      </b-card-group>
      <b-pagination
          v-model="page"
          :total-rows="rows" 
          :per-page="pages" 
        class="mt-4">
        <span class="text-danger" slot="prev-text" @click="changerPages( page - 1)" >Anterior</span>
        <span class="text-warning" slot="next-text" @click="changerPages( page + 1)">Siguiente</span>
        <span slot="page" slot-scope="{ page, active }"  @click="changerPages( page )">
          <b v-if="active">{{ page }}</b>
          <i v-else>{{ page }}</i>
        </span>
      </b-pagination>
    </b-container>
    <!-- <h1>Consultar datos</h1> -->

    <!-- <div v-for="carate of  caracterers" v-bind:key="carate.id">{{carate}}</div> -->
  </div>
</template>

<script>
// libreria
import axios from "axios";
// importamos el comnente
import Character from "./components/Character.vue";
import { constants } from 'fs';
export default {
  name: "app",
  components: {
    Character
  },
  data: function() {
    return {
      caracterers: [], // array de personajes
      page: 1,
      pages: 1,
      rows: 100,
      perPage: 10,
      currentPage: 1
    };
  },
  created() {
    this.Fetch();
  },
  methods: {
    Fetch() {
      const params = {
        page:this.page,
      } //consulta los datos  rick y morty en paginacion
      let resultado = axios
        .get("https://rickandmortyapi.com/api/character/",{params})
        .then(res => {
          this.caracterers = res.data.results;
           console.log(res.data);
          // console.log(res.data.info);
          this.pages = res.data.info.pages;
        })
        .catch(err => {
          console.log(err);
        });
    },
    changerPages(page) {
          console.log(page);
          this.Fetch();
    },
  }
};
</script>

<style>
</style>
