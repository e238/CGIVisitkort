	
<template>
    <div id="app">
        <div style="color: blue;">
            <h1>Fantastiska visitkort</h1>
            <h2>Tryck på ett för att uppdatera</h2>
        </div>
        <button class="btn btn-primary w-10 mt-5" @click="addNote()">
           skapa visitkort
        </button>
            <div class ="inside" 
            v-for="(note, index) in notes"
            :key="note.id"
            :id="index"
            @click="selectNote($event)"
            >
                <Note
                @app-removeNote="removeNote"
                @app-saveNote="saveNote"

                :note="notes[index]"
                :currentindex="index"
                />
            </div>

    </div>
</template>
 
<script>
//import NotesList from '@/components/NotesList.vue';
import Note from '@/components/Note.vue';
import firebase from "firebase/app";
import "firebase/database";
 
// Your web app's Firebase configuration
  var firebaseConfig = {
    apiKey: "AIzaSyD6nqiTEUBJIGC3qyFp92s1-9gJ1YZ8zxw",
    authDomain: "cgi-databas.firebaseapp.com",
    databaseURL: "https://cgi-databas.firebaseio.com",
    projectId: "cgi-databas",
    storageBucket: "cgi-databas.appspot.com",
    messagingSenderId: "239843102889",
    appId: "1:239843102889:web:d167961f3c64054a1719b8",
    measurementId: "G-YZZ6J3LEKT"
  };
  // Initialize Firebase
  firebase.initializeApp(firebaseConfig);
  const database = firebase.database();
  const notesRef = database.ref("notes");
 
export default {
  name: "app",
  components: {
 //   NotesList,
    Note
  },
  data: () => ({
    notes: [],
    index: 0
  }),

  methods: {
    selectNote(event){
      this.index = event.currentTarget.id;
      this.$bus.$emit('selectedNote', { ix:this.index });

      //this.$emit('selectedNote', { ix:this.index } );
    },
    addNote() {
      this.notes.unshift({
        name: "",
        surname: "",
        email: "",
        telephone: "",
        currentIndex:0,
        imgs:"data:image/gif;base64,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",
      });
      this.index = 0;
    },
     saveNote() {
       //currently selected
      const note = this.notes[this.index];
      if (note.id) {
        this.updateNote(note);
      } else {
        this.createNote(note);
      }
    },
    updateNote(note) {
      notesRef.child(note.id).update({
        name: note.name,
        surname: note.surname,
        email:note.email,
        telephone: note.telephone,
        imgs:note.imgs
      });
    },
    changeNote(index) {
      this.index = index;
    },
    createNote(note) {
      note.id = notesRef.push(note).key;
    },
    removeNote() {
      /*
      this.notes.splice(this.index, 1);
      this.index = this.index === 0 ? 0 : this.index - 1;
      */
      const id = this.notes[this.index].id;
      notesRef.child(id).remove();
    },
  },
  created() {

      notesRef.once("value", notes => {
        notes.forEach(note => {
          this.notes.push({
            id: note.ref.key,
            name: note.child("name").val(),
            surname: note.child("surname").val(),
            email: note.child("email").val(),
            telephone: note.child("telephone").val(),
            imgs:note.child("imgs").val()
          });
        });
      });

    notesRef.on("child_added", snapshot => {
      console.log("note was added: ", { ...snapshot.val(), id: snapshot.key });
    });

    notesRef.on("child_removed", snapshot => {
      const deletedNote = this.notes.find(note => note.id === snapshot.key);
      console.log("note was removed: ", deletedNote);
 
      const index = this.notes.indexOf(deletedNote);
      this.notes.splice(index, 1);
      this.index = this.index === 0 ? 0 : index - 1;
    });

    notesRef.on("child_changed", snapshot => {
      const updatedNote = this.notes.find(note => note.id === snapshot.key);
      updatedNote.title = snapshot.val().title;
      updatedNote.content = snapshot.val().content;
      console.log("note was updated: ", updatedNote);
    });
  }
};

/* eslint-disable no-console */
    // value = snapshot.val() | id = snapshot.key

</script>
<style>
#app {
  display: grid;
  grid-template-rows: 2r 4fr 1fr;
    justify-items: center;


}
h1{
  font-size: 4em;
}
.inside{
   /*background: transparent;*/

   border: solid rgb(165, 147, 147);
border-width: 1px 1px 1px 1px;
margin-top: 60px;
margin-bottom: 60px;
padding-bottom: 6px;
padding-top: 6px;
width: 600px;

   border-top-width: 5px;
   border-bottom-width: 5px;
    font-size:0;

}

</style>