
<template>
  <div class="container">
    <h1>Ваши заметки</h1>

    <div class="container__buttons">
      <button class="add-note__title" @click="addNote">
        <img class="note-add-image" src="../images/note-add.png" />
      </button>

      <div class="search">
        <input
          v-model="inputStr"
          class="search__input"
          placeholder="Начнем поиск по ключевым словам!"
        />
        <button class="add-note__title">
          <img class="search__img" src="../images/search.png" />
        </button>
      </div>
    </div>
    <div class="notes">
      <note
        class="note"
        v-for="(note, id) of searchNotes"
        :key="note.id"
        :note="note"
        @tagChange="tagChange(id, $event)"
        @noteChange="noteChange(id, $event)"
        @deleteNote="deleteNote"
      />
    </div>
  </div>

  <!-- <a href="https://www.flaticon.com/ru/free-icons/">Блокнот иконки от Vectors Market - Flaticon</a> -->
</template>

<script>
import note from "./note";

export default {
  name: "notes-vue",
  components: { note },
  data() {
    return {
      inputStr: "",
      notes: [],
    };
  },
  methods: {
    deleteNote(note) {
      let updateNotes = this.notes.filter((el) => el.id !== note.id);
      this.notes = updateNotes;
    },
    addNote() {
      let ID = this.notes.length + 1;
      this.notes.push({ title: "Заметка", body: "", tag: [], id: ID });
    },
    noteChange(id, newText) {
      this.notes[id].body = newText;
    },
    tagChange(id, newTag) {
      this.notes[id].tag.push(newTag);
    },
  },
  computed: {
    searchNotes() {
      if (this.inputStr) {
        return this.notes?.filter((el) => {
          return ( el.tag?.includes(this.inputStr.toLowerCase()) || el.title?.includes(this.inputStr.toLowerCase()) || el.body?.includes(this.inputStr.toLowerCase()));
        });
      }
      return this.notes;
    },
  },
  mounted() {
    this.notes = JSON.parse(localStorage.getItem("notes")) || [];
  },
  watch: {
    notes: {
      handler(notes, oldValue) {
        console.log(notes, oldValue);
        localStorage.setItem("notes", JSON.stringify(notes));
      },
      deep: true,
    },
  },
};
</script>

<style>
input::-webkit-input-placeholder { color: #be935a; }
h1 {
  color: #591c12;
}
.note-add-image:hover{
  width: 60px;
  transition-duration: 250ms;
  transition-timing-function: linear;
}
.container__buttons {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 90%;
}

.search {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.search__input {
  border-radius: 5px;
  width: 225px;
  margin-right: 10px;
  height: 30px;
  border: 1.5px solid #be935a;
  background-color: #faf6d2;
  font-size: 12px;
 color: #be935a;
  outline: none;
}
.search__img {
  width: 40px;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 20px;
}
.notes {
  list-style-type: none;
  display: flex;
  flex-direction: row;
  justify-content: center;
  flex-wrap: wrap;
}
.note {
  background-color: #ffff00;
  border-radius: 8px;
  width: 280px;
  margin: 0 10px 20px;
  box-shadow: 1px 3px 5px rgba(0, 0, 0, 0.2);
  transition: all 0.5s;
  cursor: pointer;
  height: 100%;
  min-height: 170px;
}

.note:active {
  position: relative;
  top: 1px;
}

.add-note__title {
  border: none;
  border-radius: 16px;
  padding: 10px 5px 10px;
  background-color: #faf6d2;
}

.note-add-image {
  width: 50px;
}
</style>
