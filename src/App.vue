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
          <form action="">
            <label for="search">Cerca il tuo film preferito:</label>
            <input class="search_bar" type="text" id="search" name="search" placeholder="Inserisci un film" v-model='searchText'>
            <button @click.prevent='searchMovie' class="btn">Cliccami Guascone</button>
          </form>
         
        </div>
      </div>
    </div>
    <div class="mostra_lista">
      <ul>
        <li>Titolo: </li>
        <li>Titolo originale: </li>
        <li>Lingua: </li>
        <li>Voto: </li>
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
      url: "https://api.themoviedb.org/3/search/movie?api_key=dab9404e3203e7f2da900e8cf30bd5e8&language=en-US&page=1&include_adult=false&query=ritorno al futuro",
      movies: null,
      loading: true,
      error: null,
      searchText: '',
    };
  },
  methods: {
    searchMovie() {
      console.log('...Ricerca');
    },
    callApi() {
      axios.get(this.url).then(response => {
        console.log(this);
        console.log(response);

        this.movies = response.data.response
      })
      .catch(error => {
        console.log(error);
      })
    }
  }, 
  mounted() {
    this.callApi();
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
