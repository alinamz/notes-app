<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div
    class="hashtag">{{ "#" + hash }}</div>
</template>

<script>

export default {
   props: {
    hash: {
        type: Object,
        required: true,
    }
   },
   data() {
    return {
      newTag: this.hash,
    };
  }
}

</script>

<style scoped>
.hashtag {
    font-size: 10px;
    color: 	#8B4513;
    background-color: rgba(250,0,0,0.2);
    line-height: 20px;
    height: 20px;
    border-radius: 10px;
    padding-right: 5px;
    padding-left: 5px
}
</style>