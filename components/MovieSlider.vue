<template lang="html">
  <flickity ref="flickity" class="carousel" :options="flickityOptions">
    <div v-for="(movie, i) in movies" :key="i" class="carousel__cell">
      <div class="carousel__cell-inner">
        <img
          v-if="hasValue(movie.Poster)"
          class="carousel__cell-img"
          :src="movie.Poster"
          :alt="movie.Title"
          @load="onImageLoad"
        />
        <img
          v-if="!hasValue(movie.Poster)"
          class="carousel__cell-img"
          src="~/assets/media/image-not-found.jpg"
          alt=""
          @load="onImageLoad"
        />

        <div class="carousel__cell-info">
          <div class="carousel__cell-info-inner">
            <div class="carousel__cell-info-item">
              <span class="carousel__cell-info-title">Title:</span>
              {{ movie.Title }}
            </div>
            <div class="carousel__cell-info-item">
              <span class="carousel__cell-info-title">Director:</span>
              {{ hasValue(movie.Director) ? movie.Director : 'Unknown' }}
            </div>
            <div class="carousel__cell-info-item">
              <span class="carousel__cell-info-title">Year:</span>
              {{ movie.Year }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </flickity>
</template>

<script>
export default {
  props: {
    movies: {
      type: Array,
      required: true,
      default: () => [],
    },
  },

  data() {
    return {
      flickityOptions: {
        draggable: true,
        imagesLoaded: true,
        groupCells: true,
        cellAlign: 'left',
        contain: true,
      },

      unknownValue: 'N/A',
    }
  },

  watch: {
    movies: {
      immediate: true,
      handler(newVal, oldVal) {
        this.$nextTick(() => {
          this.$refs.flickity.rerender()
        })
      },
    },
  },

  methods: {
    hasValue(val) {
      return String(val) !== this.unknownValue
    },

    onImageLoad() {
      this.$refs.flickity.resize()
    },
  },
}
</script>

<style lang="scss" scoped>
.carousel {
  width: 100%;

  &__cell {
    position: relative;
    display: flex;
    flex-direction: column;
    width: 100%;
    margin: 0 1%;
  }

  &__cell-inner {
    width: 100%;
    border-radius: 10px;
    overflow: hidden;
    margin: 0 auto;
  }

  &__cell-img {
    display: block;
    width: 100%;
  }

  &__cell-info {
    width: 100%;
    background-color: $black-08;
    transition: opacity 0.2s linear;
  }

  &__cell-info-inner {
    color: $white;
    padding: 20px;
  }

  &__cell-info-item + &__cell-info-item {
    margin-top: 10px;
  }

  &__cell-info-title {
    font-weight: bold;
  }

  @media screen and (max-width: $screen-sm-max) {
    &__cell-inner {
      max-width: 280px;
    }
  }

  @media screen and (min-width: $screen-md-min) {
    &__cell {
      width: 32%;
      margin-left: 1.5%;
      margin-right: 0;
    }

    &__cell-info {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      opacity: 0;
      background-color: $black-08;
      transition: opacity 0.2s linear;
    }
  }

  @media screen and (min-width: $screen-lg-min) {
    &__cell {
      width: 23%;
      margin-left: 2.4%;
      margin-right: 0;
    }
  }

  @media (hover: hover) {
    &__cell:hover &__cell-info {
      opacity: 1;
    }
  }
}
</style>
