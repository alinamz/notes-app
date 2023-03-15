<template>
  <div v-bind:class="[show ? 'container' : '']">
    <burger
      :listTags="listTags"
      :show="show"
      @changeShow="changeShow"
      @showAllNotes="showAllNotes"
      @filteringNotes="filteringNotes"
    ></burger>

    <div v-bind:class="[show ? 'container__grid' : '']">
      <h1>Ваши заметки</h1>

      <div class="container__buttons">
        <button class="add-note__title add-note__title_note" @click="addNote">
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
          @changeBackground="changeBackground"
          @tagChange="tagChange(id, $event)"
          @noteChange="noteChange(id, $event)"
          @deleteNote="deleteNote"
        />
      </div>
    </div>
  </div>

  <!-- <a href="https://www.flaticon.com/ru/free-icons/">Блокнот иконки от Vectors Market - Flaticon</a> -->
</template>

<script>
import note from "./note";
import burger from "./burger-menu.vue";

export default {
  name: "notes-vue",
  components: { note, burger },
  data() {
    return {
      inputStr: "",
      notes: [],
      show: false,
      listTags: [],
      tag: "",
      allNotes: true,
    };
  },
  methods: {
    deleteNote(note) {
      let updateNotes = this.notes.filter((el) => el.id !== note.id);
      this.notes = updateNotes;
    },
    addNote() {
      let ID = this.notes.length + 1;
      this.notes.push({
        title: "Заметка",
        body: "",
        tag: [],
        id: ID,
        color: "",
      });
    },
    noteChange(id, newText) {
      this.notes[id].body = newText;
    },
    tagChange(id, newTag) {
      this.notes[id].tag.push(newTag);
      if (Object.values(this.listTags).includes(newTag)) {
        return this.listTags;
      } else {
        this.listTags.unshift(newTag);
      }
    },
    changeBackground(id, background) {
      this.notes.forEach((element) => {
        if (element.id === id) {
          element.color = background;
        }
      });
    },
    changeShow(show) {
      this.show = show;
    },
    filteringNotes(value) {
      this.tag = value;
      this.allNotes = false;
    },
    showAllNotes() {
      this.allNotes = true;
    },
  },
  computed: {
    searchNotes() {
      if (this.allNotes == false) {
        if (this.tag) {
          let a = this.notes?.filter((el) => el.tag?.includes(this.tag));
          if (this.inputStr) {
            return a?.filter((el) => {
              return (
                el.tag?.includes(this.inputStr.toLowerCase()) ||
                el.title?.includes(this.inputStr.toLowerCase()) ||
                el.body?.includes(this.inputStr.toLowerCase())
              );
            });
          } else {
            return a;
          }
        } else if (this.inputStr) {
          return this.notes?.filter((el) => {
            return (
              el.tag?.includes(this.inputStr.toLowerCase()) ||
              el.title?.includes(this.inputStr.toLowerCase()) ||
              el.body?.includes(this.inputStr.toLowerCase())
            );
          });
        }
      } else if (this.inputStr) {
        return this.notes?.filter((el) => {
          return (
            el.tag?.includes(this.inputStr.toLowerCase()) ||
            el.title?.includes(this.inputStr.toLowerCase()) ||
            el.body?.includes(this.inputStr.toLowerCase())
          );
        });
      } else {
        return this.notes;
      }
      return this.notes;
    },
  },
  mounted() {
    this.notes = JSON.parse(localStorage.getItem("notes")) || [];
    this.listTags = JSON.parse(localStorage.getItem("tagsList")) || [];
  },
  watch: {
    notes: {
      handler(notes, oldValue) {
        console.log(notes, oldValue);
        localStorage.setItem("notes", JSON.stringify(notes));
        localStorage.setItem("tagsList", JSON.stringify(this.listTags));
      },
      deep: true,
    },
  },
};
</script>

<style>
input::-webkit-input-placeholder {
  color: #be935a;
}
h1 {
  color: #591c12;
}
.note-add-image:hover {
  width: 60px;
  transition-duration: 250ms;
  transition-timing-function: linear;
}
.container__buttons {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin: 0;
  width: 100%;
  height: 80px;
}

.container {
  display: flex;
  flex-flow: row wrap;
}

.search {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.container__grid {
  display: flex;
  flex-direction: column;
  width: 80%;
  margin-left: auto;
}

.add-note__title_note {
  margin-left: 50px;
  cursor: pointer;
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

.search {
  margin-right: 50px;
}

.notes {
  list-style-type: none;
  display: flex;
  flex-direction: row;
  justify-content: center;
  flex-wrap: wrap;
  grid-area: main;
}
.note {
  border-radius: 8px;
  width: 280px;
  margin: 0 10px 20px;
  box-shadow: 1px 3px 5px rgba(0, 0, 0, 0.2);
  transition: all 0.5s;
  cursor: pointer;
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
