<template>
  <li
    :class="[
      'letter',
      {'letter--is-in-word': isInWord },
      {'letter--is-in-place': isInPlace }
    ]"
  >
    {{ character }}
  </li>
</template>

<script setup>
const props = defineProps({
  character: String,
  isInWord: Boolean,
  isInPlace: Boolean
})
</script>

<style lang="scss">
  .letter {
    background: gray;
    color: gray;
    font-size: 2rem;
    font-weight: bold;
    padding: 1rem;

    &--is-in-word {
      background: yellow;
      color: black;
    }

    &--is-in-place {
      background: green;
      color: white;
    }
  }
</style>