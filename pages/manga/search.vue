<template>
  <div>
      <div>
    <v-text-field
    v-model="query"
    label="Search"
    ></v-text-field>
    <v-btn @click="handleSearchManga">
      Search
    </v-btn>
    </div>
    <v-divider class="mt-2 mb-2"></v-divider>
    <div class="d-flex flex-wrap">
    <v-card
    v-for="manga in result" 
    :key="manga.mal_id"
    class="ma-2"
    max-width="344"
    outlined
    @click="handleMangaClick(manga)"
  >
    <v-list-item three-line>
      <v-list-item-content>
        <div class="text-overline mb-4">
          {{ manga.volumes }}
        </div>
        <v-list-item-title class="text-h5 mb-1">
           {{ manga.title }}
        </v-list-item-title>
        <v-list-item-subtitle>
            {{ manga.synopsis }}
        </v-list-item-subtitle>
      </v-list-item-content>

      <v-list-item-content>
          <img :src="manga.image_url" :style="{width: '80px', marginTop:'5px'}" />
      </v-list-item-content>
    </v-list-item>
  </v-card>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    name:'SearchGG',
    asyncData(ctx){
      const url = `https://api.jikan.moe/v3/search/manga?q=${ctx.route.query.q}&page=1`
      return axios.get(url).then((res)=>{
        return{
          query: ctx.route.query.q,
          results: res.data.results
        }
      })
    },
    data(){
        return{
            query: '',
            results:[]
        }
    },
    mounted(){
      console.error('MOUNTED RESULTS', this.results);
    },
    methods:{
        handleSearchManga(){
          this.$router.replace({name: 'manga-search', query: {q: this.query}})
            const url = `https://api.jikan.moe/v3/search/manga?q=${this.query}&page=1`
            axios.get(url).then((res)=>{
                console.log(res.data);
                this.results = res.data.results
            })
        },
        handleMangaClick(manga){
            console.log('manga',manga);
            window.location = manga.url
        }
    },
}
</script>

<style>

</style>