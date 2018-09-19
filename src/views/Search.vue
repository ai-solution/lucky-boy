<template>
  <div class="searchWrapper">
    <div class="search">
      <label for="search">Search</label>
      <input 
        id="search" 
        name="search" 
        v-model="searchQuery"
        @input="handleInput" />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';
import debounce from 'lodash.debounce';
import axios from 'axios';

const API = 'https://images-api.nasa.gov/search';

export default {
  name: 'search',
  components: {
    HelloWorld,
  },
  data() {
    return {
      searchQuery: '',
      results: [],
    };   
  },
  methods: {
    handleInput: debounce(function() {
      axios.get(`${API}?q=${searchQuery}&media_type=image`)
      .then((response) => {
        this.results = response.data.collection.items;
      })
      .catch((error) => {
        console.log(error);
      });   
    }, 500),
  },
};
</script>

<style lang="scss" scoped>
  .searchWrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 0;
    padding: 30px;
    width: 100%;
  }

  .search {
    display: flex;
    flex-direction: column;
    text-align: center;
  }

  input {
    border: 0;
    border-bottom: 1px solid black;
  }

  label {
    font-family: 'Montserrat', sans-serif;
  }
</style>

