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
      <hr />
    </div>
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
