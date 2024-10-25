<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);
const editingNote = ref(null);

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

// Cargar notas de localStorage
if (localStorage.getItem("notes")) {
  notes.value = JSON.parse(localStorage.getItem("notes"));
}

const addNote = () => {
  if (newNote.value.length < 10) {
    errorMessage.value = "Notas con un mínimo de 10 letras";
    return;
  } 
  if (newNote.value.length > 80) {
    errorMessage.value = "Notas con un máximo de 80 letras";
    return;
  }
  // Si hay una nota en edición, actualiza su contenido
  if (editingNote.value !== null) {
    const noteIndex = notes.value.findIndex(note => note.id === editingNote.value);
    if (noteIndex !== -1) {
      notes.value[noteIndex].text = newNote.value;
    }
    editingNote.value = null;
  } else {
    notes.value.push({
      id: Math.floor(Math.random() * 1000000),
      text: newNote.value,
      date: new Date(),
      backgroundColor: getRandomColor(),
      showFullText: false,
    });
  }

  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
  localStorage.setItem("notes", JSON.stringify(notes.value));
};

const editNote = (note) => {
  editingNote.value = note.id;
  newNote.value = note.text;
  showModal.value = true;
};

const deleteNote = (value) => {
  alert("Vas a eliminar esta nota");
  showModal.value = false;
  notes.value.splice(value, 1);
  localStorage.setItem("gym-vue", JSON.stringify(this.ejercicios));
};

const closeModal = () => {
  showModal.value = false;
  errorMessage.value = "";
  newNote.value = "";
  editingNote.value = null;
};

</script>


<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="notes" rows="10"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <div class="btn">
          <button @click="addNote">{{ editingNote !== null ? 'Guardar cambios' : 'Añadir nota' }}</button>
          <button class="close" @click="closeModal">Cerrar</button>
        </div>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Tablón de notas</h1>
        <button @click="showModal = true; editingNote = null">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{ backgroundColor: note.backgroundColor }">
          <p class="main-text">{{ note.text }}</p>
          <div class="date">{{ new Date(note.date).toLocaleDateString("es-ES") }}
          <button class="btn-note" @click="editNote(note)">Editar</button>
          <button class="btn-note" @click="deleteNote(note)">Borrar</button></div>
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
  font-weight: bold;
  margin: 20px 5px 0 10px;
  word-wrap: break-word;
}
.date {
  font-size: 22px;
  font-style: italic;
  display: flex;
  justify-content: end;
}
.btn-note {
  width: 3rem;
  border-radius: 50px;
  border: 1px solid black;
  background-color: transparent;
  margin-left: 12px;
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
  width: 40%;
  height: 50%;
  background-color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  position: fixed;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
}
.modal textarea {
  text-indent: 8px;
  font-size: 32px;
  width: 90%;
  height: 50%;
  border: 1px solid black;
  border-radius: 10px;
  margin-bottom: 24px;
}
.btn {
  width: 60%;
  display: flex;
  justify-content: space-around;
  gap: 40px;
}
.modal button {
  padding: 10px 20px;
  font-size: 20px;
  border-radius: 8px;
  background-color: rgb(33, 142, 179);
  color: white;
  cursor: pointer;
  margin-top: 50px;
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
  margin: 0;
}

@media only screen and (max-width: 608px) {
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
  font-size: 15px;
  margin: 0;
}
.btn-note {
  width: 2.7rem;
  font-size: 8px;
  border-radius: 50px;
  border: 1px solid black;
  background-color: transparent;
  margin: 0;
}
.date {
  font-size: 12px;
  gap: 2px;
  margin-left: 2px;
}
.modal {
  width: 80%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  gap: 0px;
  margin: 0;
}
.modal textarea {
  font-size: 24px;
  width: 100%;
  padding: 0;
  margin: 0;
}
.btn {
  width: 90%;
  display: flex;
  justify-content: space-between;
  gap: 4px;
}
.modal button {
  font-size: 18px;
  width: 50%;
  height: 50%;
  padding: 8px;
  border-radius: 8px;
}
.modal p {
  font-size: 19px;
  font-weight: bold;
  color: rgb(255, 0, 0);
}
}
@media (min-width: 600px) and (max-width: 1210px) {
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
