<template>
  <div>
    <h1>Input Wordle</h1>
    <template v-if="isPuzzleSolved">
      <p type="text">PUZZLE SOLVED! 👏</p>
      <p>keyStrokes: {{ keyStrokes }}</p>
      <button @click="reset">Try again</button>
    </template>
    <template v-else>
      <p>Type something to solve the puzzle</p>
      <input v-if="!isPuzzleSolved" type="text" v-model="inputText">
    </template>
    <ul class="worlde">
      <li v-for="(word, i) in words" :key="i">
        <Word
          :word="word"
          :words="words"
          :inputText="inputText"
          @wordSolved="onWordSolved"
          :reset="resetPuzzle"
        />
      </li>
    </ul>
  </div>
</template>

<script setup>
  import { ref, watch, computed, nextTick } from 'vue'
  import Word from './components/Word.vue'

  const words=[
    'peace',
    'ocean',
    'waves',
    'house',
    'vuejs'
  ]

  const solvedWords = ref([])

  const inputText=ref('')
  let keyStrokes = ref(0)
  let resetPuzzle = ref('')

  const isPuzzleSolved = computed(() => {
    return solvedWords.value.length === 5
  })

  const onWordSolved = word => solvedWords.value.push(word)

  const reset = () => {
    solvedWords.value = []
    inputText.value = ''
    resetPuzzle.value = ''
    nextTick(() => { // watched prop in child component would not update otherwise
      resetPuzzle.value = 'reset'
    })
  }

  watch(
    () => inputText.value,
    () => keyStrokes.value++
  )

</script>

<style lang="scss">

  ul {
    list-style: none;
    margin: 0;
    padding: 0;
  }

  input {
    margin-bottom: 1rem;
  }

  button {
    margin-bottom: 1rem;
  }

</style>
