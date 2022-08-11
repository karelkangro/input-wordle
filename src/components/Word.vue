<template>
  <ul
    :class="[
      'wordle-word',
      {'wordle-word--is-solved': isWordSolved },
    ]"
  >
    <Letter
      v-for="(letter, index) in props.word" :key="index"
      :character="letter"
      :isInWord="isLetterInWord(props.inputText, letter)"
      :isInPlace="isLetterInRightPlace(props.word, index, props.inputText)"
    />
  </ul>
</template>

<script setup>
import { ref, watch } from 'vue'
import Letter from './Letter.vue'

const isLetterInWord = (word, letter) => word.includes(letter)
const isLetterInRightPlace = (word, index, inputText) => word.split('')[index] === inputText.split('')[index]

const emit = defineEmits(['wordSolved'])
const props = defineProps({
  word: String,
  words: {
    type: Object,
    default() {
      return []
    }
  },
  inputText: String,
  reset: String
})

let isWordSolved = ref('')

watch(
  () => props.inputText,
  text => compareWordsAndSetIsWordSolved(text, props.word)
)

watch(
  () => props.reset,
  () => isWordSolved.value = false
)

const compareWordsAndSetIsWordSolved = (userInputText, currentWord) => {

  if(isWordSolved.value) return // prevent emitting again

  isWordSolved.value = userInputText === currentWord

  if(isWordSolved.value) emit('wordSolved', props.word)
}

</script>

<style lang="scss">
  .wordle-word {
    display: grid;
    grid-template-columns: 3fr 3fr 3fr 3fr 3fr;
    grid-gap: 1rem;
    margin-bottom: 1rem;

    &--is-solved {
      li {
        background: green;
        color: white;
      }
    }
  }
</style>