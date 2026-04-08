<template>
  <div class="TextStudy auto-overflow">
    <div class="p-2 i-theme-color">
      <h1>Study</h1>
      <input ref="searchInput" @change="onInputChange" />
    </div>
    <div class="p-2 auto-overflow">
      <div v-for="word in data.words" :key="word.word" class="p-1 float-right">
        <table class=" text-center" style="float: right">
          <tbody>
            <tr>
              <td class="hebrew">{{ word.word }}</td>
            </tr>
            <tr>
              <td><small @click="onWordClicked" class="clickable">{{ word.transliteration }}</small></td>
            </tr>
            <tr>
              <td><small @click="onWordClicked" class="clickable">{{ word.root }}</small></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'TextStudy',
  props: {
    msg: String
  },
  data() {
    return {
      data: {}
    }
  },
  mounted() {
    this.search('Genesis 1:1')
  },
  methods: {
    search(part) {
      this.$refs.searchInput.value = part;
      axios.get('http://localhost:8000/?searchString='+part)
         .then(response => this.data = response.data)
        // .then(response => console.log(response))
    },
    onInputChange() {
      if (event) {
        this.search(event.target.value);
      }
    },
    onWordClicked() {
      // `event` is the native DOM event
      if (event) {
        this.$emit('wordClicked', event.target.textContent);
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hebrew {
  font-size: 1.5em;
}

.clickable:hover {
  cursor: pointer;
  background-color: yellow;
}
</style>
