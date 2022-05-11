<template>
  <div id="app">
    <header id="site_header">
      <nav id="site_nav">
        <div class="container-fluid bg-dark">
            <div class="row row-cols-2 align-items-center">
              <div class="col">
                      <div class="logo">
                        <img src="@/assets/img/giovannielloflix.png" alt="">
                      </div>
              </div>
              <div class="col d-flex justify-content-end">
                  <form action="" autocomplete="off">
                        <label class="text-white me-3" for="search">Cerca il tuo contenuto preferito:</label>
                        <input class="search_bar border-light border_radius_05 bg-dark" type="text" id="search" name="search" placeholder="Inserisci film o serie" v-model='searchMovie'>
                        <button @click.prevent="callMyApi" class="btn border_radius_05">Cerca</button>
                  </form>
              </div>
            </div>
        </div>
      </nav>
    </header>

    <main id="site_main">
         <div class="mostra_lista">
             <h2>Film:</h2>
             <div class="container">
               <div class="row row-cols-3 gy-3">
                 <div class="col" v-for='movie in movies' :key='movie.id'>
                   <div class="card">
                     <img v-if="movie.poster_path == null" src="https://picsum.photos/342/500" alt="" >
                     <img v-else :src="'https://image.tmdb.org/t/p/w342/' + movie.poster_path" alt="">
                   </div>
                   <div class="text">
                      <div class="title">Titolo: {{movie.title}} </div>
                      <div class="original_title">Titolo originale: {{movie.original_title}} </div>
                      <div class="language">Lingua: {{movie.original_language}}  <flag :iso="languageFlag(movie.original_language)" /></div>
                      <div class="vote">Voto: {{Math.ceil(parseInt(movie.vote_average) / 2)}} </div>
                      <div class="starsVote d-flex">
                        <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 1 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 3 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 5 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 7 ? 'star_show' : 'star_standard'"/>
                        <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 9 ? 'star_show' : 'star_standard'"/>
                      </div>
                      <div class="overview"> Overview: {{movie.overview}}</div>
                   </div>
                 </div>
               </div>
             </div>

<!--               <ul v-for='movie in movies' :key='movie.id'>
                <li>
                  <img v-if="movie.poster_path == null" src="https://picsum.photos/342/500" alt="" >
                  <img v-else :src="'https://image.tmdb.org/t/p/w342/' + movie.poster_path" alt="">
                </li>
                <li>Titolo: {{movie.title}} </li>
                <li>Titolo originale: {{movie.original_title}} </li>
                <li>Lingua: {{movie.original_language}}  <flag :iso="languageFlag(movie.original_language)" /></li>
                <li>Voto: {{Math.ceil(parseInt(movie.vote_average) / 2)}} </li>
                <div class="starsVote d-flex">
                  <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 1 ? 'star_show' : 'star_standard'"/>
                  <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 3 ? 'star_show' : 'star_standard'"/>
                  <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 5 ? 'star_show' : 'star_standard'"/>
                  <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 7 ? 'star_show' : 'star_standard'"/>
                  <font-awesome-icon icon="fa-solid fa-star" :class="movie.vote_average >= 9 ? 'star_show' : 'star_standard'"/>
                </div>
              

              
              
              </ul> -->
         </div>
    </main>
    


     <div class="mostra_lista">
       <h2>Serie Tv:</h2>
      <ul v-for='show in shows' :key='show.id'>
        <li>
          <img v-if="show.poster_path == null" src="https://picsum.photos/342/500" alt="">
          <img v-else :src="'https://image.tmdb.org/t/p/w342/' + show.poster_path" alt="">
        </li>
        <li>Titolo: {{show.name}} </li>
        <li>Titolo originale: {{show.original_name}} </li>
        <li>Lingua: {{show.original_language}}  <flag :iso="languageFlag(show.original_language)" /></li>
        <li>Voto: {{Math.ceil(parseInt(show.vote_average) / 2)}} </li>
        <div class="starsVote d-flex">
          <font-awesome-icon icon="fa-solid fa-star" :class="show.vote_average >= 1 ? 'star_show' : 'star_standard'"/>
          <font-awesome-icon icon="fa-solid fa-star" :class="show.vote_average >= 3 ? 'star_show' : 'star_standard'"/>
          <font-awesome-icon icon="fa-solid fa-star" :class="show.vote_average >= 5 ? 'star_show' : 'star_standard'"/>
          <font-awesome-icon icon="fa-solid fa-star" :class="show.vote_average >= 7 ? 'star_show' : 'star_standard'"/>
          <font-awesome-icon icon="fa-solid fa-star" :class="show.vote_average >= 9 ? 'star_show' : 'star_standard'"/>
        </div>
       
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {
   
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
        console.log(this);
        console.log(response);
        this.movies = response.data.results
        this.searchMovie= ''
      })
      .catch(error => {
        console.log(error);
      }),

       axios.get(this.urlTv + this.searchMovie).then(response => {
        console.log(this);
        console.log(response);
        this.shows = response.data.results
        this.searchMovie= ''
      })
      .catch(error => {
        console.log(error);
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
