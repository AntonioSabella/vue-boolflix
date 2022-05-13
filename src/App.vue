<template>
  <div id="app">
    <header id="site_header">
      <nav id="site_nav" class="py_1">
        <div class="container-fluid">
            <div class="row row-cols-2 align-items-center h-100 flex-nowrap">
              <div class="col">
                <LogoComponent />
                    
              </div>
              <div class="col d-flex justify-content-end">
                  <form class="w-75 d-flex justify-content-end" action="" autocomplete="off" @submit.prevent="callMyApi">
                        <!-- <label class="text-white me-3" for="search">Cerca il tuo contenuto preferito:</label> -->
                        <input class="search_bar border-light border_radius_05 bg-dark w-80 me-3" type="text" id="search" name="search" placeholder="Inserisci film o serie" v-model='searchMovie'>
                        <button :disabled='searchMovie.length < 1' class="btn border_radius_05 bg-dark">Cerca</button>
                  </form>
              </div>
            </div>
        </div>
      </nav>
    </header>

    <main id="site_main" class="mb-4">
         <div class="mostra_lista p-4">
             <div class="container-fluid"> 
               <h2 class="text-center">Film</h2>
               <div class="row row-cols-5 pb-2 flex-nowrap overflow-auto">
                 <div class="col p-0" v-for='movie in movies' :key='movie.id'>
                   <div class="poster">
                     <img class="rounded img-fluid"  v-if="movie.poster_path == null" src="https://picsum.photos/342/513" alt="" >
                     <img class="rounded img-fluid" v-else :src="'https://image.tmdb.org/t/p/w342/' + movie.poster_path" alt="">
                   </div>
                   <div class="text">
                      <div class="title"><strong>Titolo</strong>: {{movie.title}} </div>
                      <div class="original_title"><strong>Titolo originale</strong>: {{movie.original_title}} </div>
                      <div class="language"><strong>Lingua</strong>: {{movie.original_language}}  <flag :iso="languageFlag(movie.original_language)" /></div>
                      <div class="vote"><strong>Voto</strong>: {{Math.ceil(parseInt(movie.vote_average) / 2)}} </div>
                      <div class="starsVote d-flex">
                        <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 1 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 3 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 5 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 7 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 9 ? 'star_show' : 'star_standard'"/>
                      </div>
                      <div class="overview"><strong>Trama</strong>: {{movie.overview}}</div>
                   </div>
                 </div>
               </div>
             </div>
         </div>
         <div class="mostra_lista p-4">
              <div class="container-fluid"> <!-- Container fluid crea problemi con la width della row degli show -->
                <h2 class="text-center">Serie Tv</h2>
               <div class="row row-cols-5 pb-2 flex-nowrap overflow-auto">
                 <div class="col fixed_height p-0" v-for='show in shows' :key='show.id'>
                   <div class="poster fixed_height fixed_width">
                     <img class="rounded img-fluid" v-if="show.poster_path == null" src="https://picsum.photos/342/513" alt="" >
                     <img class="rounded img-fluid" v-else :src="'https://image.tmdb.org/t/p/w342/' + show.poster_path" alt="">
                   </div>
                   <div class="text fixed_height fixed_width">
                      <div class="title"><strong>Titolo</strong>: {{show.name}} </div>
                      <div class="original_title"><strong>Titolo originale</strong>: {{show.original_name}} </div>
                      <div class="language"><strong>Lingua</strong>: {{show.original_language}}  <flag :iso="languageFlag(show.original_language)" /></div>
                      <div class="vote"><strong>Voto</strong>: {{Math.ceil(parseInt(show.vote_average) / 2)}} </div>
                      <div class="starsVote d-flex">
                        <font-awesome-icon icon="fa-solid fa-star" :class="show.vote_average >= 1 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="show.vote_average >= 3 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="show.vote_average >= 5 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="show.vote_average >= 7 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="show.vote_average >= 9 ? 'star_show' : 'star_standard'"/>
                      </div>
                      <div class="overview"><strong>Trama</strong>: {{show.overview}}</div>
                   </div>
                 </div>
               </div>
             </div>
         </div>

    </main>
    


  </div>
</template>

<script>
import LogoComponent from '@/components/LogoComponent.vue'
import axios from 'axios';

export default {
  name: 'App',
  components: {
   LogoComponent
  },
  data() {
    return {
      url:'https://api.themoviedb.org/3/search/movie?api_key=dab9404e3203e7f2da900e8cf30bd5e8&language=it-IT&page=1&include_adult=false&query=?',
      urlTv: 'https://api.themoviedb.org/3/search/tv?api_key=dab9404e3203e7f2da900e8cf30bd5e8&language=it-IT&page=1&include_adult=false&query=?',
      searchMovie: '',
      movies: null,
      shows: null,
      error: null,
    };
  },
  methods: {
    callMyApi(){
      axios.get(this.url + this.searchMovie).then(response => {
        //console.log(this);
        //console.log(response);
        this.movies = response.data.results
        this.searchMovie= ''
      })
      .catch((error) => {
        //console.log(error);
        this.error = `Si è verificato un problema: ${error}`;
      }),

       axios.get(this.urlTv + this.searchMovie).then(response => {
        //console.log(this);
        //console.log(response);
        this.shows = response.data.results
        this.searchMovie= ''
      })
      .catch((error) => {
        //console.log(error);
        this.error = `Si è verificato un problema: ${error}`;
      })
    },
    languageFlag(flagspeak){
      if(flagspeak === 'en'){
        return flagspeak = 'gb'
      } else {
        return flagspeak
      }
    },
  }, 
  mounted() {
    this.callMyApi();
  },

}
</script>

<style lang="scss">
@import '@/assets/scss/style.scss';
</style>
