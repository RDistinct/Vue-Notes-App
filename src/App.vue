<script setup>
import { ref } from "vue";

// MODAL STATE
// First we declare the state of the modal. Then we use directives to change that state
// After that, we want to store the state of the user's input, & bind it to our input element
// Next is to store each note in an array.

const showModal = ref(false);

//const showHeading = ref(true);

const newNote = ref("");

const newNoteWarning = ref("");

const notes = ref([]);

const addNote = () => {
  //add notes if input characters are > 10
  if (newNote.value.length < 9) {
    return (newNoteWarning.value = "Note needs to be more than 10 charactera");
    //this prevents the form from continuing
    //we are trmming to get rid of whitespaces in the todo
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });

  showModal.value = !showModal; //close the modal after adding note
  newNote.value = ""; //reset the value of newNote
  newNoteWarning.value = "";
};

//generate random light color
function getRandomColor() {
  return "hsl( " + Math.random() * 360 + ", 100%,  75%)";
}
</script>

<template>
  <!-- THIS IS WHERE HTML WILL BE   -->
  <main>
    <!-- <button
      v-bind="showHeading"
      @click="showHeading = !showHeading"
      v-if="showHeading"
    >
      If TRUE, now you see me!
    </button> -->

    <div v-if="showModal" class="overlay">
      <div class="modal">
        {{ newNote }}
        <!--because newNote is 2- way binded, we dont need to use
        computed properties to keep track of letter input coz vue is already
        doing that for us. -->
        <textarea
          v-model.trim="newNote"
          name="notes"
          id="notes"
          cols="30"
          rows="10"
        ></textarea>
        <!-- we can also trim on the v-model -->
        <p v-if="newNoteWarning">{{ newNoteWarning }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = !showModal">Close</button>
      </div>
    </div>

    <div class="container">
      <header>
        <!-- {{ notes }}  {{ showModal }}-->
        <h1>Notes App</h1>
        <button @click="showModal = true">+</button>
      </header>

      <div class="cards-container">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vh;
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}
.cards-container {
  display: flex;
  /*flex-direction: row;*/
  flex-wrap: wrap;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}
h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}
header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

/** CARD  **/
.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}
.modal {
  width: 150px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}
.modal button {
  padding: 10px 20px;
  margin-top: 15px;
  font-size: 20px;
  width: 100%;
  border: none;
  color: white;
  cursor: pointer;
}
.close {
  background-color: red;
  margin-top: 15px;
}
.modal p {
  color: red;
}
</style>
