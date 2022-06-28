<template>
  <div>
    <v-text-field v-model="query" label="Search"></v-text-field>
    <v-btn @click="searchManga">Search</v-btn>
    <h1>{{result}} = result</h1>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      query: '',
      result:[]
     
    

    }
  },methods: {
    searchManga(){
     const url = `https://api.jikan.moe/v3/search/manga?q=${this.query}&page=1`;
      axios.get(url).then((res) =>{    
        this.result = res.data.result
      })
    }
  }
}
