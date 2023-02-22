<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="note-body">
    <div>
      <span class="note-body__delete" @click="deleteNote(this.note)">X</span>
    </div>
    <textarea class="note-body__title" v-model="newNote.title"></textarea>
    <textarea
      class="note-body__main"
      v-model="newNote.body"
      @click="(event) => bodyChange(event)"
      :key="note.key"
    ></textarea>
    <div class="hashtags">
      <input
        :key="note.key"
        v-on:keyup.enter="(event) => addHashtag(event)"
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
import hashtag from "./hashtag.vue";

export default {
  components: { hashtag },
  name: "note-vue",
  props: {
    note: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      newNote: this.note,
    };
  },
  methods: {
    deleteNote(newNote) {
      console.log(newNote.title)
      this.$emit("deleteNote", newNote);
    },
    bodyChange(event) {
      this.$emit("noteChange", event.target.value);
    },
    addHashtag(event) {
      if(event.target.value !== '') {
      this.$emit("tagChange", event.target.value);
      event.target.value=''
      }
    },
  },
};
</script>

<style scoped>
.note-body__title {
  font-family: "Itim", sans-serif ;
  background-color: #ffff00;
  -webkit-appearance: none;
  color: #623c18;
  width: 94%;
  height: 100%;
  border: none;
  resize: none;
  margin-top: 20px;
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
}

.hashtag-input {
  background-color: #ffff00;
  border: none;
  margin-bottom: 10px;
  width: 100px;
  height: 20px;
  font-size: 10px;
  color: #8B4513;
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
</style>
