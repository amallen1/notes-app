// Design and develop a Vue SPA – “notes-vue-app” that stores notes in an array.
2. Each note should contain title, author, year of publication and isRead of a
book. 3. The following operations have to be applicable on the notes: 1. Add a
new note. Do not add duplicate titles. 2. Remove an existing note. 3. Display if
a note is read or not using isRead flag. 4. List all notes BONUS: Search
functionality: 1. Enter title of note to search 2. Display the author and year
of publication

<template>
  <div>
    <Notes />
  </div>
</template>

<script>
import Notes from "./components/Notes.vue";

export default {
  name: "App",
  components: {
    Notes,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100vh;
}
</style>
