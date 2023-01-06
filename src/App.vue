<template>
  <div class="body">
    <div v-if="showModal" class="overlay">
      <div @keyup.escape="showModal = false" class="modal">
        <textarea
          @keyup.enter="addNote"
          v-model="newModal"
          name="note"
          id="note"
          cols="30"
          rows="10"
        >
        </textarea>
        <p class="count">{{ charCount }}/5000</p>
        <p class="error" v-if="errorMsg">{{ errorMsg }}</p>
        <button @click="addNote" class="btn-add-note">Add Note</button>
        <button @click="showModal = false" class="btn-close-note">Close</button>
      </div>
    </div>
    <div class="container">
      <div class="header">
        <h1>Notes</h1>
        <button @click="showModal = true" class="btn-add">+</button>
      </div>
      <div class="card-container">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.bgColor }"
        >
          <p class="text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
let showModal = ref(false);
let newModal = ref("");
let errorMsg = ref("");
let notes = ref([]);
let charCount = computed(()=>{
    return newModal.value.length;
})

// function ranColor() {
//   return "hsl(" + Math.random() * 360 + ", 100%, 7%)";
// }
// return ark colors

// function ranColor() {
//   // Generate random values for the red, green, and blue components of the color
//   const red = Math.floor(Math.random() * 256);
//   const green = Math.floor(Math.random() * 256);
//   const blue = Math.floor(Math.random() * 256);

//   // Calculate the luminance of the color
//   const luminance = 0.2126 * red + 0.7152 * green + 0.0722 * blue;

//   // Check if the luminance is greater than or equal to 128
//   // If it is, the color is considered light and we return it
//   if (luminance >= 128) {
//     return `rgb(${red}, ${green}, ${blue})`;
//   }

//   // If the luminance is less than 128, we generate a new color
//   return ranColor();
// }

function randColor() {
  const color = `rgb(${Math.floor(Math.random() * 256)}, ${Math.floor(
    Math.random() * 256
  )}, ${Math.floor(Math.random() * 256)})`;
  return 0.2126 * parseInt(color.slice(4, 7), 10) +
    0.7152 * parseInt(color.slice(9, 12), 10) +
    0.0722 * parseInt(color.slice(14, 17), 10) >=
    128
    ? color
    : randColor();
}

let addNote = () => {
  if (newModal.value.length <= 10) {
    return (errorMsg.value = "Note needs to be atleast 10 character.");
  }
  notes.value.push({
    id: Math.floor(Math.random() * 100),
    text: newModal.value,
    date: new Date(),
    bgColor: randColor(),
  });
  showModal.value = false;
  newModal.value = "";
  errorMsg.value = "";
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Quicksand&display=swap');

.body {
  
  background-color: #8360c3;
}
.container {
  max-width: 1000px;
  padding: 15px;
  margin: 0 auto;
}
.header {
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin-bottom: 30px;
}
h1 {
  font-weight: bold;
  color: #dff9fb;
  font-size: 45px;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
}
.btn-add {
  text-align: center;
  color: #dff9fb;
  font-weight: bolder;
  background-color: #6f1e51;
  border-radius: 50%;
  border: none;
  padding: 10px;
  width: 40px;
  height: 40px;
  font-size: 15px;
  font-weight: bold;
  cursor: pointer;
}
.card {
  background-color: rgb(191, 238, 222);
  height: 230px;
  width: 220px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
  border-radius: 10px;
  overflow: auto;
}
.card p {
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
}
.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px 10px;
}
.overlay {
  position: absolute;
  background-color: rgba(0, 0, 0, 0.77);
  height: 100%;
  width: 100%;
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
}
textarea{
  font-family: 'Quicksand', sans-serif;
}
.modal {
  display: flex;
  flex-direction: column;
  width: 700px;
  position: relative;
  background-color: rgb(210, 247, 234);
  padding: 10px;
  border-radius: 10px;
}
.error {
  color: maroon;
}
.count{
  color: blue;
}
.btn-add-note {
  padding: 10px 20px;
  margin-top: 7px;
  background-color: #39cf5a;
  font-size: 15px;
  font-weight: bold;
  cursor: pointer;
  font-family: 'Quicksand', sans-serif;
  
}
.btn-close-note {
  background-color: #e55039;
  padding: 10px 20px;
  margin-top: 7px;
  font-size: 15px;
  font-weight: bold;
  cursor: pointer;
  font-family: 'Quicksand', sans-serif;
}
</style>
