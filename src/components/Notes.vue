// Design and develop a Vue SPA – “notes-vue-app” that stores notes in an array.
2. Each note should contain title, author, year of publication and isRead of a
book. 3. The following operations have to be applicable on the notes: 1. Add a
new note. Do not add duplicate titles. 2. Remove an existing note. 3. Display if
a note is read or not using isRead flag. 4. List all notes BONUS: Search
functionality: 1. Enter title of note to search 2. Display the author and year
of publication

<template>
  <div>
    <header><h1>Notes App</h1></header>

    <main>
      <form @submit.prevent="addNote">
        <h2>Add a new note</h2>
        <label>Title</label>
        <input type="text" class="input" v-model="title" required />
        <div v-if="duplicateError" class="error">{{ error }}</div>

        <label>Author</label>
        <input type="text" class="input" v-model="author" required />

        <label>Year of Publication</label>
        <input
          type="number"
          class="input"
          v-model="pubYear"
          required
          min="0"
          max="2022"
        />

        <input type="submit" class="submit-btn input" />
        <div v-if="submitted" class="submitted">{{ message }}</div>
      </form>

      <!-- this div shows the list of notes -->
      <section class="note-list">
        <div class="search">
          <input
            type="text"
            class="search-bar"
            placeholder="Search for a title..."
            v-model="searchValue"
          />

          <button @click="listNotes()" class="list-btn">
            {{ showNotes ? "Hide Notes" : "Show Notes" }}
          </button>
        </div>

        <div v-show="showNotes" class="notes-info">
          <h2>Note List</h2>
          <p v-if="notes.length === 0" style="text-align: center">
            There are no notes currently.
          </p>
          <div
            class="note-container"
            v-for="note in filteredNotes"
            :key="note"
            v-else
          >
            <div class="note-content">
              <div>
                <p><span>Title:</span> {{ note.title }}</p>
                <p><span>Author: </span> {{ note.author }}</p>
                <p><span>Year of Publication: </span>{{ note.pubYear }}</p>
                <p><span>Read: </span>{{ note.isRead ? "Yes" : "No" }}</p>
              </div>
              <div class="buttons">
                <button @click="markAsRead(note)">Mark as Read</button>
                <button @click="deleteNote(note)">Delete</button>
              </div>
            </div>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "",
      author: "",
      pubYear: "",
      notes: [],
      isRead: false,
      showNotes: false,
      searchValue: "",
      duplicateError: false,
      error: "Duplicate titles are not accepted.",
      submitted: false,
      message: "Note added!",
    };
  },

  methods: {
    addNote() {
      //Checks for duplicate note titles
      if (this.notes.find((note) => note.title === this.title)) {
        this.duplicateError = true;
        return;
      }

      this.notes.push({
        title: this.title,
        author: this.author,
        pubYear: this.pubYear,
      });

      //Resets the form inputs after the submit button is clicked
      this.title = "";
      this.author = "";
      this.pubYear = "";
      this.submitted = true;
      this.duplicateError = false;

      setTimeout(() => {
        this.submitted = false;
      }, 2000);
    },
    deleteNote(note) {
      this.notes = this.notes.filter((item) => note !== item);
    },
    listNotes() {
      this.showNotes = !this.showNotes;
    },
    markAsRead(note) {
      note.isRead = !note.isRead;
    },
  },
  computed: {
    //allows users to search for a specific note
    filteredNotes() {
      let tempNotes = this.notes;

      if (this.searchValue !== "" && this.searchValue) {
        tempNotes = tempNotes.filter((note) => {
          return note.title.includes(this.searchValue);
        });
      }
      return tempNotes;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

body {
  background-color: rgb(203, 233, 253);
}

header {
  background-color: rgb(69, 73, 126);
  padding: 1rem;
  color: white;
}

main {
  margin-top: 5rem;
  display: flex;
}

form {
  margin: 0 8rem;
  align-self: flex-start;
}

form label {
  font-weight: 700;
}

form,
.notes-info {
  width: 50%;
  max-width: 420px;
  background-color: white;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
  box-shadow: 4px 4px 40px 0px rgba(0, 0, 0, 0.2);
}

.notes-info {
  margin-top: 2rem;
  width: 420px;
  padding: 40px;
}

.input {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
  margin: 1rem 0;
}

.submit-btn {
  transition: transform 0.2s;
  cursor: pointer;
}

.submit-btn:hover {
  transform: scale(1.05);
  background-color: #555;
  color: white;
}

button {
  cursor: pointer;
}

h2 {
  text-align: center;
  margin-bottom: 1rem;
}

.search-bar {
  width: 350px;
  background-color: white;
  border-radius: 40px;
  padding: 1rem;
  box-shadow: 0px 5px 15px 0px rgb(69 73 126 / 20%);
  border: none;
}

.search-bar:focus {
  outline: none;
}

.list-btn {
  padding: 1rem;
  border: none;
  background-color: rgb(69, 73, 126);
  color: white;
  border-radius: 10px;
  font-weight: 700;
  margin: 0 5px;
}

.list-btn:hover {
  background-color: rgb(50, 53, 90);
}

.note-container {
  background-color: rgb(224, 224, 224);
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 1rem;
}

.note-container span {
  font-weight: 700;
}

.note-content {
  display: flex;
  justify-content: space-between;
}

.note-content button {
  align-self: center;
  padding: 0.5rem;
  background-color: #555;
  border-radius: 10px;
  color: white;
  font-weight: 700;
  border: none;
  margin: 0.25rem 0;
}

.note-content button:hover {
  background-color: rgb(54, 54, 54);
}

.buttons {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: space-between;
}

input {
  margin-bottom: 0.5rem;
}

.error {
  color: #ff0062;
  margin: 10px 0;
  font-size: 0.8em;
  font-weight: bold;
}

.submitted {
  color: green;
  text-align: center;
  font-weight: bold;
}
</style>
