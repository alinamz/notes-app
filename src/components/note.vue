<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="note-body" v-bind:style="{ backgroundColor: newNote.color }">
    <div>
      <span class="note-body__delete" @click="deleteNote(this.note)">X</span>
    </div>
    <button
      v-if="showDelete"
      v-bind:style="{ backgroundColor: newNote.color }"
      v-on:mouseover="show = !show"
      class="note-body__backgrn"
    ></button>
    <backgroundList 
      @changeBackground="changeBackground"
      v-if="show"
      :key="note.key"
    ></backgroundList>
    <textarea
      v-bind:style="{ backgroundColor: newNote.color }"
      class="note-body__title"
      v-model="newNote.title"
    ></textarea>
    <textarea
      v-bind:style="{ backgroundColor: newNote.color }"
      class="note-body__main"
      v-model="newNote.body"
      @click="(event) => bodyChange(event)"
      :key="note.key"
    ></textarea>
    <div class="hashtags">
      <input
        :key="note.key"
        v-on:keyup.enter="(event) => addHashtag(event)"
        v-bind:style="{ backgroundColor: newNote.color }"
        @blur="(event) => addHashtag(event)"
        placeholder="#Новый хэштег"
        class="hashtag-input"
      />
      <div class="hashtag-all">
        <hashtag
          v-for="(hash, index) in newNote.tag"
          :key="index"
          :hash="hash"
        ></hashtag>
      </div>
    </div>
  </div>
</template>

<script>
import backgroundList from "./background-list.vue";
import hashtag from "./hashtag.vue";

export default {
  components: { hashtag, backgroundList },
  name: "note-vue",
  props: {
    note: {
      type: Object,
      required: true,
    },
    showDelete: {
      type: Boolean,
    }
  },
  data() {
    return {
      newNote: this.note,
      show: false,
    };
  },
  methods: {
    deleteNote(newNote) {
      this.$emit("deleteNote", newNote);
    },
    bodyChange(event) {
      this.$emit("noteChange", event.target.value);
    },
    addHashtag(event) {
      if (event.target.value !== "") {
        this.$emit("tagChange", event.target.value);
        event.target.value = "";
      }
    },
    changeBackground(background) {
      this.$emit("changeBackground", this.newNote.id, background);
    },
  },
};
</script>

<style >
.note-body__title {
  font-family: "Itim", sans-serif;
  background-color: #ffff00;
  -webkit-appearance: none;
  color: #623c18;
  width: 94%;
  border: none;
  resize: none;
  margin-top: 30px;
  text-align: center;
  outline: none;
  font-weight: 900;
  font-size: 19px;
}
.note-body__main {
  background-color: #ffff00;
  -webkit-appearance: none;
  width: 94%;
  min-height: 100px;
  outline: none;
  border: none;
  resize: vertical;
  display: inline-block;
  overflow: hidden;
}
.note-body__delete {
  position: absolute;
  top: 9px;
  right: 9px;
}

.note-body {
  position: relative;
  background-color: #ffff00;
}

.hashtag-input {
  background-color: #ffff00;
  border: none;
  margin-bottom: 10px;
  width: 100px;
  height: 20px;
  font-size: 10px;
  color: #8b4513;
}
.hashtag-input:focus {
  outline: #623c18;
}
.hashtags {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-left: 7px;
}

.hashtag-all {
  display: flex;
  flex-direction: row;
  gap: 12px;
  margin-right: 10px;
  flex-wrap: wrap;
}

.note-body__backgrn {
  position: absolute;
  top: 8px;
  left: 8px;
  width: 18px;
  height: 18px;
  outline: none;
  border: 3px;
  border-color: #ffffff;
  border-radius: 9px;
  border-style: solid;
  background-color: #fbec5d;
}
</style>
