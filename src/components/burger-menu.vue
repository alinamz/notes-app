<template>
  <link
    rel="stylesheet"
    href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@48,400,0,0"
  />
  <nav class="burger" :class="[hidden ? 'active' : '']">
    <transition name="fade" mode="out-in">
      <span
        v-if="!hidden"
        @click="onClick"
        key="menu"
        class="material-symbols-outlined menu"
      >
        menu
      </span>
      <span
        v-else
        @click="onClick"
        key="clear"
        class="material-symbols-outlined close"
      >
        close
      </span>
    </transition>
    <transition name="fade">
      <ul v-if="hidden" class="list">
        <div @click="showAllNotes" class="allNotes">&#128161; ЗАМЕТКИ</div>
        <li class="tag" @click="filteringList(hash)" v-for="(hash, index) in listTags"
          :key="index"
          :hash="hash">
          <img src="../images/hash.png" alt="hashtag" class="hash__img"/>
          {{ hash }}
        </li>
        <div class="basket" @click="hiddenDeletedNotes"> &#x1f5d1; КОРЗИНА</div>
      </ul>
    </transition>
  </nav>
</template>

<script>

export default {
   props: {
    show: {
      type: String,
      required: true,
    },
    listTags: {
      type: Object,
      required: true,
    }
},
  data() {
    return {
    hidden: this.show,
    items: this.listTags
    };
  },
  methods: {
    onClick() {
      this.hidden = !this.hidden;
      this.$emit('changeShow', this.hidden);
    },
    filteringList(hash) {
     this.$emit('filteringNotes', hash)
    },
    showAllNotes() {
      this.$emit('showAllNotes')
    },
    hiddenDeletedNotes() {
      this.$emit('hiddenDeletedNotes')
    }
  }
};
</script>

<style scoped>

.tag {
  cursor: pointer;
}
.tag:hover {
  opacity: 0.5;
}
.allNotes, .basket {
  padding-top: 10px;
  padding-bottom: 10px;
  width: 97%;
  background-color: #bfbb97;
  margin-bottom: 10px;
  text-align: initial;
  padding-left: 5px;
  cursor: pointer;
}

.allNotes:hover, .basket:hover {
  opacity: 0.8;
}
.hash__img {
  width: 20px;
  height: 20px;
}
.active {
  background-color: #e1ddbd;
}
.list {
  position: relative;
  top: 15px;
  margin: 0;
  padding: 0;
  list-style: none;
  width: 200px;
  flex-flow: column;
  align-items: baseline;
  left: 0px;
  display: flex;
  gap: 12px;
  font-size: 22px;
  color: #591c12;
  height: 100vh;
}

.menu,
.close {
  font-size: 40px;
  top: -263px;
  width: 20%;
}

.burger {
  position: absolute;
  top: 0px;
}

.menu {
  margin-left: 30px;
}
.close {
  margin-right: 69px;
}
</style>