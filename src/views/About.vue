<template>
  <div class="about">
    <h1>This is an about page</h1>

    <section class="section">
    <!-- Form to search for movie by title -->
    <label class="label">Azure API Test</label>
    
    <div class="field">
      <div class="control">
        <input class="input" type="text" placeholder="Text Input" v-model="query">
      </div>
      <div class="control">
        <a class="button is-info" @click="fetchMovie">Search</a>
      </div>
    </div>
    <!-- Notification containing info from a successful API call -->
    <div class="notification" v-if="typeof movie.stream_service != 'undefined'">
      <h1>{{ movie.stream_service[0] }}</h1>
      <div class="level-item title">{{ movie.stream_service }} {{ movie.poster }} 
        <div v-for="service in movie.stream_service" :key="service">
          <p class="subtitle">{{ service }}</p>
        </div>
        <img v-bind:src="movie.poster" v-bind:alt="movie.title">
      </div>

      <hr>
    </div>
    <div class="container">
      <div class="columns">
        <div class="column is-quarter"></div>
        <div class="column is-half">
          <img v-bind:src="testRequest[0].poster" :alt="testRequest[0].title">
          
        </div>
        <div class="column is-quarter"></div>
      </div>
    </div>
    </section>

    <article class="media">
      <figure class="media-left">
        <p class="image">
          <img v-bind:src="testRequest[0].poster" :alt="testRequest[0].title">
        </p>
      </figure>
      <div class="media-content">
        <div class="content">
          <h1 class="title"> {{ testRequest[0].title }}</h1>
          <p>These are the providers you can watch on for free with a subscription:</p>
          <div v-for="service in testRequest[0].stream_service" :key="service.title">
            <p>{{service}}</p>
          </div>
        </div>
      </div>
    </article>

  </div>
</template>

<script>

export default {
  data: () => ({
    query: "",
    movie: {},
    url_base: "http://summerstream.azurewebsites.net/index.php",
    testRequest: [
      {
        title: "Terminator 2",
        stream_service: ["HBO", "Netflix"],
        poster: "https://resizing.flixster.com/Cv2EPiuODhgnnvyzXNDUzt4pK4o=/206x305/v1.bTsxMjMyNTU1MTtqOzE4NTAyOzEyMDA7MjIwMDsyOTM0"
      }
    ]
  }),
  methods: {
    fetchMovie () {
      fetch(`${this.url_base}?movie_title=${this.query}`)
      .then(res => res.json())
      .then(body => this.setResults(body));
    },
    setResults (results) {
      this.movie = results;
    }
  }
}
</script>
