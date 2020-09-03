<template>
<div class="note">

            <img :src="note.imgs" alt="no image"/>
         <div class="col-12">
          <div class="input-group input-group-lg w-60 mx-auto">
                <div class="input-group-prepend">
                    <span class="input-group-text">Namn</span>
                </div>
                <input type="text" class="form-control" v-model="note.name" placeholder="Namn">
          </div>
         </div>
         <div class="col-12">
          <div class="input-group input-group-lg w-60 mx-auto">
                <div class="input-group-prepend">
                    <span class="input-group-text">Efternamn</span>
                </div>
            <input class="form-control" type="text" v-model="note.surname" placeholder="Efternamn"/>
          </div>
         </div>
         <div class="col-12">
          <div class="input-group input-group-lg w-60 mx-auto">
                <div class="input-group-prepend">
                    <span class="input-group-text">Email</span>
                </div>
            <input class="form-control" type="email" v-model="note.email" placeholder="jan@example.com"/>
          </div>
         </div>
         <div class="col-12">
          <div class="mb-2 input-group input-group-lg w-60 mx-auto">
                <div class="input-group-prepend">
                    <span class="input-group-text">Telefon</span>
                </div>
            <input class="form-control" type="tel" v-model="note.telephone" placeholder="00000000"/>
          </div>
         </div>
    <!--<img class="viewimage" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAAUA
    AAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO
    9TXL0Y4OHwAAAABJRU5ErkJggg==" alt="Red dot" />
    -->

        <div class="buttonholder" :style="{visibility: disp ? 'visible' : 'hidden'}">

            <button class="ml-3 mt-2 btn btn-success my-auto"
            @click="saveNote()">Spara</button>
            <button class="ml-2 btn btn-danger my-auto"
            @click="removeNote()">Ta bort</button>
            <label class="ml-2 mb-2 btn btn-primary my-auto"  
            >
            Byt bild <input type="file" @change=uploadImage hidden>
            </label>

        </div>
</div>

        
</template>
 
<script>
//import PictureInput from 'vue-picture-input'
export default {
  name: "Note",
  computed: "false",
  /*
  components:{
    PictureInput
  },
  */
  props: ["note","currentindex","disp"],
  methods: {
    removeNote() {
      this.$emit("app-removeNote");
    },
    toggleSelected(ix){
        console.log(ix);
        console.log(this.currentindex);

    },
saveNote() {
      this.$emit("app-saveNote");
    },

    uploadImage(e){
                const image = e.target.files[0];
                const reader = new FileReader();
                reader.readAsDataURL(image);
                reader.onload = e =>{
                    this.imgstring = e.target.result;
                    this.note.imgs = e.target.result;
                    console.log(this.imgstring);
                };
            },
      onChange () {
        console.log("Picture changed!")
      }
  },
  created(){
      this.disp=false;
       this.$bus.$on('selectedNote', ($event) => {
           if(this.currentindex==$event.ix){
               this.disp = true;
           }
           else{
               this.disp= false;
           }
    })
  }

};
</script>
 
<style scoped>
.note {
width: 400px;
height: 200px;
}
*, *:before, *:after {
  box-sizing: border-box;
}
img{
    width: 100px;
    height: 100px;
}
.note{
    width: 100%;
    height: 100%;
    box-shadow: 0 3px 5px 0 rgba(0,0,0,0.08);

}
.note:hover{
  box-shadow: 0 10px 25px 6px rgba(0, 0, 0, 0.1);
}

.buttonholder{
    align-items: center;
    justify-items: center;

}
.hide{
    visibility: hidden;
}
.show{
    display: inline;
}
</style>