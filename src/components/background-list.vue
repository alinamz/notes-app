<template>
  <ul class="background-list">
    <li class="background-list__el">
      <button @click="onClick('#98ff98')" style="background-color: #98ff98" class="button__bckgrnd button__green"></button>
    </li>
    <li class="background-list__el">
      <button @click="onClick('#ffc0cb')" style="background-color: #ffc0cb" class="button__bckgrnd button__bckgrnd_pink"></button>
    </li>
    <li class="background-list__el">
      <button @click="onClick('#ffffff')" style="background-color: #ffffff" class="button__bckgrnd button__bckgrnd_white"></button>
    </li>
    <li class="background-list__el">
      <button @click="onClick('#ff6347')" style="background-color: #ff6347" class="button__bckgrnd button__bckgrnd_red"></button>
    </li>
    <li class="background-list__el">
      <button @click="onClick('#ffff00')" style="background-color: #ffff00" class="button__bckgrnd button__bckgrnd_red"></button>
    </li>
  </ul>
</template>

<script>
export default {
  props: {},
  data() {
    return {
        background : this.background
    }
  },
  methods: {
    onClick(background) {
        this.$emit("changeBackground", background);
    }
  }
};
</script>

<style>
.background-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: row;
  gap: 7px;
  position: absolute;
  left: 32px;
  top: 5px;
}

.button__bckgrnd {
  width: 18px;
  height: 18px;
  outline: none;
  border: 3px;
  border-color: #ffffff;
  border-radius: 9px;
  border-style: solid;
}
</style>