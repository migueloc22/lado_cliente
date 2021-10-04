<template>
  <!-- <div v-for="carate of  caracterers" v-bind:key="carate.id"> -->
  <div>
    <div v-bind:key="character.id">
      <b-card
        v-bind:title="character.name"
        v-bind:img-src="character.image"
        img-alt="Image"
        img-top
      >
        <b-button variant="outline-danger" @click="showModal(character.id)">Detalles</b-button>
      </b-card>
    </div>
    <b-modal ref="my-modal" hide-footer v-bind:title='titu'>
          
          <h4>Left and Right (or Start and End)</h4>
         <!-- <img  v-bind:img-src='image' width="20%" alt="" srcset="">  -->
         <b-img v-bind:src='image' rounded alt="Rounded image"></b-img>
          <!-- <b-button class="mt-3" variant="outline-danger" block @click="hideModal">Close Me</b-button>
          <b-button class="mt-2" variant="outline-warning" block @click="toggleModal">Toggle Me</b-button> -->
    </b-modal>
  </div>
  
</template>

<script>
import axios from "axios";
export default {
  /*
    Los props son la comunicacion de padre el hijo

 */
  data() {
        return {
            titu: 'tiutlo',
            image: 'https://placekitten.com/300/300' ,
            obj : {}
        }
    },
  props: ["character"],
  methods:{
    async showModal(id) {
      
      
    //   let resultado = axios.get("https://rickandmortyapi.com/api/character/"+id);

    //  let obj ={};
    //  obj= resultado.data;
     let resultado = await axios
        .get("https://rickandmortyapi.com/api/character/"+id)
        .then(res => {
          // this.caracterers = res.data.results;
          this.obj = res.data;
          //  console.log(res.data);
          // console.log(res.data.info);
          // this.pages = res.data.info.pages;
        })
        .catch(err => {
          console.log(err);
        });
    //  console.log(obj);
          // console.log(this.obj.length);
          this.titu="Detalles de "+this.obj.name;
           this.image=this.obj.image;
          this.$refs['my-modal'].show()
      },
      hideModal() {
        this.$refs['my-modal'].hide()
      },
      toggleModal() {
        // We pass the ID of the button that we want to return focus to
        // when the modal has hidden
        this.$refs['my-modal'].toggle('#toggle-btn')
      }
  }
};
</script>