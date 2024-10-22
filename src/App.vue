<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value =
      "Escribe al menos 10 palabras para añadir una nota");
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });
  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
};
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
        <div v-if="showModal" class="modal">
          <textarea
            v-model.trim="newNote"
            name="note"
            id="notes"
            rows="10"
          ></textarea>
          <p v-if="errorMessage">{{ errorMessage }}</p>
          <button @click="addNote">Añadir nota</button>
          <button class="close" @click="showModal = false">Cerrar</button>
        </div>
    </div>
    <div class="container">
      <header>
        <h1>Tablón de notas</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">{{ note.text }}</p>
          <div class="date">{{ note.date.toLocaleDateString("es-ES") }}</div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.container {
  max-width: 1100px;
  padding: 10px;
  margin: 0 auto;
}
header {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}
.cards-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
.card {
  width: 225px;
  height: 225px;
  background-color: rgba(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}
h1 {
  font-weight: bold;
  font-size: 75px;
}
header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: black;
  color: white;
  border-radius: 100%;
  font-size: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.main-text {
  font-size: 26px;
  margin: 20px 5px 0 10px;
}
.date {
  font-size: 22px;
  font-style: italic;
  display: flex;
  justify-content: end;
}
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7); 
  z-index: 10; 
 }
.modal {
  width: 30%;
  height: 30%;
  background-color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  position: fixed;
  top: 40%;        /* Posiciona el modal al 50% desde arriba */
  left: 50%;       /* Posiciona el modal al 50% desde la izquierda */
  transform: translate(-50%, -50%); /* Ajusta para centrarlo perfectamente */
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1); /* Añade un pequeño sombreado */
  padding: 20px;
}
.modal textarea {
  width: 90%;
  border: 1px solid black;
  border-radius: 10px;
  margin-bottom: 24px;
}
.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 30%;
  border-radius: 8px;
  background-color: rgb(33, 142, 179);
  color: white;
  cursor: pointer;
  margin-top: 15px;
}
.modal .close {
  background-color: rgb(255, 0, 0);
  color: black;
  font-weight: 700;
}
.modal p {
  font-size: 19px;
  font-weight: bold;
  color: rgb(255, 0, 0);
}

@media only screen and (max-width: 600px) {
h1 {
  margin-bottom: 45px;
  font-size: 40px;
}
header button {
  padding: 0;
  width: 28px;
  height: 28px;
  margin-bottom: 10px;
  font-size: 25px;
}
.card {
  width: 130px;
  height: 130px;
  }
.main-text {
  font-size: 16px;
  margin: 0;
}
.date {
  font-size: 14px;
}
.modal {
  width: 80%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
}
.modal textarea {
  width: 100%;
  padding: 0;
  margin: 0;
}
.modal button {
  font-size: 16px;
  width: 40%;
  border-radius: 8px;
}
.modal p {
  font-size: 19px;
  font-weight: bold;
  color: rgb(255, 0, 0);
}
}
@media (min-width: 600px) and (max-width: 1100px) {
h1 {
  font-size: 50px;
  margin-bottom: 45px;
}
.modal {
  width: 80%;
}
.modal textarea {
  width: 100%;
  padding: 0;
  margin: 0;
}
}
</style>
