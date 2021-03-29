<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <ul>
      <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
    </ul>
    <p>
      Title:
      <input type="text" v-model="newMovieTitle" placeholder="Enter Title" />
    </p>
    <p>
      Director:
      <input type="text" v-model="newMovieDirector" placeholder="Enter Director" />
    </p>
    <p>
      Genre:
      <input type="text" v-model="newMovieGenre" placeholder="Enter Genre" />
    </p>
    <p>
      Plot:
      <input type="text" v-model="newMoviePlot" placeholder="Enter Plot" />
    </p>
    <p>
      Starring:
      <input type="text" v-model="newMovieStarring" placeholder="Enter Starring" />
    </p>
    <p>
      Year:
      <input type="text" v-model="newMovieYear" placeholder="Enter Year" />
    </p>
    <p><button v-on:click="createMovie">Create Movie</button></p>
    <h3>Movie List</h3>
    <div v-for="movie in movies" v-bind:key="movie.id">
      <p>{{ movie.title }}</p>
      <button v-on:click="showMovie(movie)">More info</button>
      <hr />
    </div>
    <dialog id="movie-details">
      <form method="dialog">
        <p>
          Title:
          <input type="text" v-model="currentMovie.title" />
        </p>
        <p>
          Director:
          <input type="text" v-model="currentMovie.director" />
        </p>
        <p>
          Plot:
          <input type="text" v-model="currentMovie.plot" />
        </p>
        <p>
          Year:
          <input type="text" v-model="currentMovie.year" />
        </p>
        <button v-on:click="updateMovie(currentMovie)">Update</button>
        <button>Close</button>
        <button v-on:click="destroyMovie(currentMovie)">Destroy</button>
      </form>
    </dialog>
  </div>
</template>
<style></style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to the Movie App!",
      movies: [],
      errors: [],
      newMovieTitle: "",
      newMovieDirector: "",
      newMovieGenre: "",
      newMoviePlot: "",
      newMovieStarring: "",
      newMovieYear: "",
      currentMovie: {},
    };
  },
  created: function () {
    this.indexMovies();
    this.createMovie();
  },
  methods: {
    indexMovies: function () {
      console.log("index");
      axios.get("http://localhost:3000/api/movies").then((response) => {
        console.log(response.data);
        this.movies = response.data;
      });
    },
    createMovie: function () {
      console.log("create");
      var params = {
        title: this.newMovieTitle,
        director: this.newMovieDirector,
        genre: this.newMovieGenre,
        plot: this.newMoviePlot,
        starring: this.newMovieStarring,
        year: this.newMovieYear,
      };
      axios.post("http://localhost:3000/api/movies", params).then((response) => {
        console.log(response.data);
        this.movies.push(response.data);
        this.newMovieTitle = "";
        this.newMovieDirector = "";
        this.newMovieGenre = "";
        this.newMoviePlot = "";
        this.newMovieStarring = "";
        this.newMovieYear = "";
      });
    },

    showMovie: function (theMovie) {
      console.log(theMovie);
      this.currentMovie = theMovie;
      document.querySelector("#movie-details").showModal();
    },
    updateMovie: function (theMovie) {
      console.log(theMovie);
      var params = {
        title: theMovie.title,
        plot: theMovie.plot,
        director: theMovie.director,
        year: theMovie.year,
      };
      axios.patch("/api/movies/" + theMovie.id, params).then((response) => {
        console.log(response.data);
      });
    },
    destroyMovie: function (theMovie) {
      console.log(theMovie);
      axios.delete("/api/movies/" + theMovie.id).then((response) => {
        console.log(response.data);
        var index = this.movies.indexOf(theMovie);
        this.movies.splice(index, 1);
      });
    },
    submit: function () {
      var params = {
        title: this.newTitle,
      };
      axios
        .post("/api/movies", params)
        .then((response) => {
          console.log("Success", response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
