<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col">
          <div class="logo">
            <img src="@/assets/img/giovannielloflix.png" alt="">
          </div>
        </div>
        <div class="col d_flex">
          <form action="" autocomplete="off">
            <label for="search">Cerca il tuo film preferito:</label>
            <input class="search_bar" type="text" id="search" name="search" placeholder="Inserisci un film" v-model='searchMovie'>
            <button @click.prevent="callMovieApi" class="btn">Cliccami Guascone</button>
          </form>
         
        </div>
      </div>
    </div>
    <div class="mostra_lista">
       <h2>Film:</h2>

      <ul v-for='movie in movies' :key='movie.id'>
        <li>Titolo: {{movie.title}} </li>
        <li>Titolo originale: {{movie.original_title}} </li>
        <li>Lingua: {{movie.original_language}}  <flag :iso="languageFlag(movie.original_language)" /></li>
        <li>Voto: {{Math.ceil(movie.vote_average)}} </li>
       
      </ul>
    </div>
     <div class="mostra_lista">
       <h2>Serie Tv:</h2>
      <ul v-for='show in shows' :key='show.id'>
        <li>Titolo: {{show.name}} </li>
        <li>Titolo originale: {{show.original_name}} </li>
        <li>Lingua: {{show.original_language}}  <flag :iso="languageFlag(show.original_language)" /></li>
        <li>Voto: {{Math.ceil(show.vote_average)}} </li>
       
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
    callMovieApi(){
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
    }
  }, 
  mounted() {
    this.callMovieApi();
  },

}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

ul {
  line-height: 1.5rem;
  margin-bottom: 1rem;
  background-color: coral;
}

.mostra_lista {
  margin: 2rem;
  padding: 1rem;
}

.container {
  padding: 2rem;
  margin: auto;
  text-align: center;
  max-width: 100%;
  height: 100px;
  background-color: black;
  .row{
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
}

.search_bar {
  padding: 0.5rem 0.8rem;
  background-color: lightblue;
  margin-right: 0.5rem;
}

.btn {
  background-color: cornflowerblue;
  color: crimson;
  border: 1px solid red;
  padding: 0.5rem 0.8rem;
  cursor: pointer;
}

/* Utilities */
.d_flex {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
