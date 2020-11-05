<template>
  <div class="container container--center">
    <div>
      <h1 class="title">Movie Finder</h1>

      <SearchForm input-type="text" name="movie-search" label="Movie name" />
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 70px;
  color: #35495e;
  letter-spacing: 1px;
  margin-bottom: 32px;
}
</style>
