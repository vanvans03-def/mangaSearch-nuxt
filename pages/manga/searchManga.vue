<template>
  <div>
    <div>
      <v-text-field v-model="searchQuery" label="Search"></v-text-field>
      <v-btn @click="searchManga">Search</v-btn>
    </div>
    <v-divider class="mt-2 mb-2"></v-divider>
    <div class="d-flex flex-wrap  pa-2">
    <v-card
      v-for="manga in results"
      :key="manga.mal_id"
      class="mx-2 mt-4"
      max-width="450"
      outlined
      @click="handleMangaClick(manga)"
    >
      <v-list-item three-line>
        <v-list-item-content>
          <div class="text-overline mb-4">ตอนที่ : {{manga.chapters}}</div>
          <v-list-item-title class="text-h5 mb-1">
            {{ manga.title }}
          </v-list-item-title>
          <v-list-item-subtitle>{{ manga.synopsis }}</v-list-item-subtitle>
          <v-list-item-subtitle
            >วันที่เริ่มฉาย : {{  shortDate(manga.start_date) }}</v-list-item-subtitle
          >
          <v-list-item-subtitle v-if="manga.end_date <= 0"
            >ยังดำเนินการฉายอยู่</v-list-item-subtitle
          >
          <v-list-item-subtitle v-else
            >วันที่ฉายครั้งสุดท้าย :
            <span>{{ shortDate(manga.end_date) }}</span>
          </v-list-item-subtitle>
        </v-list-item-content>

        <img class="mt-10" :src="manga.image_url" alt="" />
      </v-list-item>
    </v-card>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import moment from "moment";
export default {
  data() {
    return {
      searchQuery: '',
      results: [],
    }
  },
  methods: {
    searchManga() {
      const url = `https://api.jikan.moe/v3/search/manga?q=${this.searchQuery}&page=1`
      axios.get(url).then((response) => {
        console.log(response.data)
        this.results = response.data.results
      })
    },
    shortDate(mangadate) {
      const date = mangadate
      return moment(date).utc().format('YYYY-MM-DD')
      
    },
    handleMangaClick(manga){
      window.open(manga.url, '_blank');
    }
  },
}
</script>

<style scoped>
img {
  width: 80px;
  margin-top: '10px';
}
</style>