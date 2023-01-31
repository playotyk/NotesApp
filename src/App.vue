<script setup>
import {ref} from "vue";

const showModal = ref(false)
const newNote = ref("")
const errorMessage = ref("")
const notes = ref([])
const placeHolder = ref("Type your note")

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (newNote.value.length < 3) {
    return errorMessage.value = "Note needs to be 3 characters or more"
  }
  notes.value.push({
    id: Math.floor(Math.random() * 10000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor()
  });
  showModal.value = false;
  newNote.value = '';
  errorMessage.value = '';
}
const startDrag = (event, item) => {
  event.dataTransfer.dropEffect = 'move';
  event.dataTransfer.effectAllowed = 'move';
  event.dataTransfer.setData('id', note.id)
}

</script>

<template>
  <main>
    <transition name="bounce">
      <div class="overlay" v-if="showModal">

        <div class="modal">
          <textarea
              v-model.trim="newNote"
              name="note"
              id="note"
              cols="30"
              rows="10"
              style="font-size: 20px; resize: none"
              @keypress.enter.prevent="addNote"
              :placeholder="placeHolder"
          ></textarea>
          <p
              v-if="errorMessage"
              :style="{color: 'red'}"
          >{{ errorMessage }}</p>
          <button
              class="add-note-button"
              @click="addNote"
          >Add Note
          </button>
          <button class="close-button" @click="showModal=false">Close</button>
        </div>

      </div>
    </transition>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button
            @click="showModal=true"
            class="add-button"
        >+
        </button>
      </header>
      <div class="cards-container">
        <div
            v-for="note in notes"
            :key="note.id"
            class="card"
            :style="{backgroundColor: note.backgroundColor}"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date"> {{ note.date.toLocaleDateString() }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}
.main-text{
  font-size: 18px;
  margin: 5px;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

.add-button {
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
.add-button:hover{
  background-color: #332e2e;
}

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

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
  -webkit-animation: scale-up-tr 0.4s cubic-bezier(0.390, 0.575, 0.565, 1.000) both;
  animation: scale-up-tr 0.4s cubic-bezier(0.390, 0.575, 0.565, 1.000) both;
}

@keyframes scale-up-tr {
  0% {
    -webkit-transform: scale(0.5);
    transform: scale(0.5);
    -webkit-transform-origin: 100% 0%;
    transform-origin: 100% 0%;
  }
  100% {
    -webkit-transform: scale(1);
    transform: scale(1);
    -webkit-transform-origin: 100% 0%;
    transform-origin: 100% 0%;
  }
}

.add-note-button{
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: #00ffff;
  border: none;
  color: black;
  cursor: pointer;
  margin-top: 15px;
  border-radius: 10px;
}

.add-note-button:hover{
  background-color: #08e8de;
  color: white;
  -webkit-animation: shadow-drop-center 0.4s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
  animation: shadow-drop-center 0.6s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
}
@-webkit-keyframes shadow-drop-center {
  0% {
    -webkit-box-shadow: 0 0 0 0 rgba(0, 0, 0, 0);
    box-shadow: 0 0 0 0 rgba(0, 0, 0, 0);
  }
  100% {
    -webkit-box-shadow: 0 0 20px 0px rgba(0, 0, 0, 0.35);
    box-shadow: 0 0 20px 0px rgba(0, 0, 0, 0.35);
  }
}
@keyframes shadow-drop-center {
  0% {
    -webkit-box-shadow: 0 0 0 0 rgba(0, 0, 0, 0);
    box-shadow: 0 0 0 0 rgba(0, 0, 0, 0);
  }
  100% {
    -webkit-box-shadow: 0 0 20px 0px rgba(0, 0, 0, 0.35);
    box-shadow: 0 0 20px 0px rgba(0, 0, 0, 0.35);
  }
}


.close-button:hover {
  background-color: #ff4040;
  -webkit-animation: shadow-drop-center 0.4s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
  animation: shadow-drop-center 0.6s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
  color: black;
}

.close-button{
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  border: none;
  color: white;
  cursor: pointer;
  border-radius: 10px;
  background-color: #ff2400;
  margin-top: 7px;
  border-radius: 10px;
}

@-webkit-keyframes scale-down-ver-center {
  0% {
    -webkit-transform: scaleY(1);
    transform: scaleY(1);
  }
  100% {
    -webkit-transform: scaleY(0.3);
    transform: scaleY(0.3);
  }
}

@keyframes scale-down-ver-center {
  0% {
    -webkit-transform: scaleY(1);
    transform: scaleY(1);
  }
  100% {
    -webkit-transform: scaleY(0.3);
    transform: scaleY(0.3);
  }
}


.bounce-leave-active {
  animation: bounce-in 0.25s reverse;
}

@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.25);
  }
  100% {
    transform: scale(1);
  }
}

</style>