<template>
<div class="jumbotron jumbotron-fluid">
  <div class="container text-center">
    <img class="w-25 mt-5" src="../assets/Quran.png">
    <h1 class="display-4">Quran-app</h1>
  </div>
</div>
  <div class="">
    <div class="container">
    <div class="row align-item-center justify-content-center">
      <form class="mt-5">
        <input class="form-control form-control-lg mb-1 font-italic" type="text" v-model="search" placeholder="cari surat..." />
      </form>
    </div>
  </div>
    <div class="d-inline-flex flex-wrap justify-content-center">
    <div class="card m-4 rounded-circle text-center shadow p-3 mb-5 bg-body rounded"  style="width: 15rem;" v-for="listChapter in cariSurah" :key="listChapter.id">
      <div class="card-body">
        <h6 class="card-title">{{ listChapter.name_simple }}</h6>
        <p class="card-text fw-lighter ">{{listChapter.revelation_place}} <br>{{listChapter.verses_count}} ayat</p>
        <router-link :to="{name: 'surah', params: {id:listChapter.id} }" class="btn btn-dark">Baca Surah</router-link>
      </div>
    </div>
  </div>
  </div>
</template>
<script>
import axios from 'axios'
import { ref } from 'vue'
const urlSurah = 'https://api.quran.com/api/v4/chapters?language=id'
export default {
  data() {
    return {
      surah: ref([]),
      search: ref('')
    }
  },
  mounted() {
    this.getSurah()
  },
  methods: {
    getSurah() {
      axios.get(urlSurah)
        .then((response) => {
          this.surah = response.data.chapters
        }).catch((error) => {
          console.log('error' + error);
        })
    }
  },
  computed: {
    cariSurah: function() {
      return this.surah.filter(surah => {
        return surah.name_simple
          .toLowerCase()
          .split("-")
          .join(" ")
          .match(this.search);
      });
    }
  }
}
</script>