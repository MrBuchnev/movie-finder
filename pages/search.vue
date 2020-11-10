<template>
  <div class="container">
    <h1 class="title">Your results:</h1>

    <client-only>
      <div v-if="!areMoviesReady" class="centered">
        <div v-if="!failedToLoad" class="loading">Loading...</div>

        <div v-if="failedToLoad">
          Unable to load movies. Try again with another movie title.
        </div>
      </div>

      <movie-slider v-if="areMoviesReady" :movies="movies" />

      <button class="back-btn" type="button" @click="goBack">Back</button>
    </client-only>
  </div>
</template>

<script>
export default {
  data() {
    return {
      receivedQuery: this.$route.query.keyword,
      movies: [],
      areMoviesReady: false,
      failedToLoad: false,
    }
  },

  mounted() {
    this.fetchAllMovies()

    setTimeout(() => {
      this.failedToLoad = true
    }, 4000)
  },

  methods: {
    // prettier-ignore
    async fetchAllMovies() {
      const parsedQuery = this.receivedQuery.replace(/-/g, ' ')
      const moviesInitialList = await (await fetch(`${process.env.apiUrl}&s=${parsedQuery}`)).json()
      const moviesIdList = (moviesInitialList.Search || []).map((movie) => movie.imdbID)
      const moviesUrlList = moviesIdList.map((movieId) => `${process.env.apiUrl}&i=${movieId}`)
      const moviesPromisesList = moviesUrlList.map((movieUrl) => fetch(movieUrl))
      const moviesResponseList = await Promise.all(moviesPromisesList)
      this.movies = await Promise.all(moviesResponseList.map((res) => res.json()))

      if (!this.movies.length) {
        this.failedToLoad = true
        return
      }

      this.areMoviesReady = true
    },

    goBack() {
      this.$router.push('/')
    },
  },
}
</script>

<style lang="scss" scoped>
.centered {
  margin: 0 auto;
}

.loading {
  animation: fade-animation 1s infinite;
}

.back-btn {
  width: 130px;
  font-size: 14px;
  line-height: 32px;
  border-radius: 8px;
  background-color: $black-05;
  color: $white;
  margin: 60px auto 0;
  transition: background-color 0.1s linear;
}

@keyframes fade-animation {
  0% {
    opacity: 0;
  }

  50% {
    opacity: 1;
  }

  100% {
    opacity: 0;
  }
}

@media (hover: hover) {
  .back-btn:hover {
    background-color: $black;
  }
}
</style>
