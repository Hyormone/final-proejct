<template>
  <div @click="goToDetail" class="gallery-item" tabindex="0">
    <img :src="review?.movie_backdrop_path ? 'https://image.tmdb.org/t/p/original' + review?.movie_backdrop_path : 'https://image.tmdb.org/t/p/original' + review?.movie_poster_path" class="gallery-image" alt="">
    <div class="gallery-item-info">
      <ul>
        <div class="mx-auto my-0 text-center">
          <li class="gallery-item-likes">{{ review?.movie_title }}</li>
          <div class="mx-auto my-0 text-center">
            <li class="gallery-item-likes"><i class="fas fa-heart" aria-hidden="true"></i><span class="me-4"> {{ likeCount }}</span></li>
            <li class="gallery-item-comments"><i class="fas fa-comment" aria-hidden="true"></i> 댓글개수</li>
          </div>
        </div>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProfileReviewListItem',
  props: {
    review: Object,
  },
  computed: {
    likeCount() {
      return this.review.like.length
    }
  },
  methods: {
    goToDetail() {
      // console.log('클릭', id)
      this.$router.push({ name: 'DetailView', params: { movie_id: this.review.movie }})
    }
  },
}
</script>

<style>

</style>