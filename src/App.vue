<script setup>
import { ref } from 'vue';

const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);

const openAndCloseModal = () => {
  showModal.value = !showModal.value;
  errorMessage.value != "" ? errorMessage.value = "" : "";
}

function getRandomColor(){
  return `hsl(${Math.floor(Math.random() * 360)}, 100%, 75%)`;  
}

const addNote = () => {
  if(newNote.value === ""){
    
    errorMessage.value = "Please enter a note!";
    return;
  } else {
    errorMessage.value = "";
    notes.value.push({
      id: notes.value.length + 1,
      text: newNote.value,
      date: new Date().toLocaleDateString(),
      backgroundColor: getRandomColor()
    });
    newNote.value = "";
    showModal.value = false;
  }
}

</script>

<template>
  <main>
    <div class="overlay" v-if="showModal">
      <div class="modal">
        <textarea name="note" id="note" cols="30" rows="10" v-model="newNote"></textarea>
        <p class="error-message" v-if="errorMessage">{{errorMessage}}</p>
        <button class="modal-button" @click="addNote">Add Note</button>
        <button class="modal-close-button" @click="openAndCloseModal">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button class="header-button" @click="openAndCloseModal">+</button>
      </header>
      <div class="cards-container">
        <div class="card" v-for="note in notes" :key="note.id" :style="{backgroundColor: note.backgroundColor}">
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">{{note.date}}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main{
    height: 100vh;
    width: 100vw;    
  }

  .container{
    display: flex;
    flex-direction: column;
    height: 100%;
    margin: 0 auto;
    max-width: 1000px;
    width: 100%;
  }

  header{
    align-items: center;
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;
  }

  h1{
    font-size: 4.6rem;
    font-weight: 700;
  }

  .header-button{
    background-color: rgb(21, 20, 20);
    border-radius: 100%;
    border: none;
    color: #fff;
    cursor: pointer;
    font-size: 1.25rem;
    height: 50px;
    width: 50px;
  }

  .cards-container{
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    max-height: 720px;
    overflow-y: auto;
    padding: 20px 0;
    scrollbar-width: thin;
    scrollbar-color: #141414 transparent;
  }

  .card{
    background-color: rgb(237, 182, 44);
    border-radius: 15px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    display: flex;
    flex-direction: column;
    height: 225px;
    justify-content: space-between;
    padding: 10px;
    width: 225px;
  }

  .main-text{
    color: #212121;
    text-overflow: ellipsis;
    white-space: wrap;
    margin-bottom: 10px;
    overflow-y: auto;
    scrollbar-width: thin;
    scrollbar-color: #000 transparent;
  }

  .date{
    color: #212121;
    font-size: 0.75rem;
    font-weight: 700;
  }

  .overlay{
    align-items: center;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    height: 100%;
    justify-content: center;
    position: absolute;
    width: 100%;
    padding: 0 20px;
  }

  .modal {
    background-color: #fff;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    padding: 20px;
    max-width: 700px;
    width: 100%;
  }

  .error-message{
    color: rgb(191, 33, 33);
    font-size: 0.9rem;
    font-weight: 700;
    margin: 1px 0;
  }

  .modal-button{
    background-color: rgb(104, 2, 155);
    border-radius: 5px;
    border: none;
    color: #fff;
    cursor: pointer;
    font-size: 1rem;
    text-transform: uppercase;
    margin-top: 10px;
    padding: 10px;
    transition: background-color 0.3s ease-in-out;
  }

  .modal-close-button{
    background-color: rgb(255, 0, 0);
    border-radius: 5px;
    border: none;
    color: #fff;
    cursor: pointer;
    font-size: 1rem;
    text-transform: uppercase;
    margin-top: 10px;
    padding: 10px;
    transition: background-color 0.3s ease-in-out;
  }

  .modal-button:hover{
    background-color: rgb(104, 2, 155, 0.9);
  }

  .modal-close-button:hover{
    background-color: rgba(255, 0, 0, 0.7);
  }

  textarea{
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 1rem;
    margin-bottom: 10px;
    padding: 10px;
  }

  @media (max-width: 1060px){
    header{
      padding: 0 10px;
    }
    .cards-container{
      padding: 0 10px;
    }
  }

  @media (max-width: 510px){
    
    h1{
      font-size: 4rem;
    }

    .header-button {
      height: 46px;
      width: 46px;
    }

    .cards-container{
      justify-content: center;
      padding: 0;
    }
  }

</style>