<template>
  <div class="home">
    <section class="hero is-medium has-text-centered mb-6">
      <div class="hero-body">
        <p class="title is-spaced is-size-1 has-text-primary has-text-centered mb-6">
          <span class="is-family-cursive">Welcome to Mosh Mart!</span>
        </p>
        <p class="is-family-cursive">
          Discover your style with the latest trends at the best price!
        </p>
      </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
        <div class="is-family-cursive">
          <h2 class="is-size-2 has-text-centered has-text-black">Our newest collection</h2>
        </div>
      </div>

      <ProductBox
        v-for="product in latestProducts"
        :key="product.id"
        :product="product"
      />
    </div>

    <!-- Review section -->
    <section class="section">
      <div class="container">
        <h3 class="is-size-3 has-text-centered is-family-cursive has-text-black">Post a Review</h3>
        <div class="field">
          <label class="label">Your Review:</label>
          <div class="control">
            <textarea class="textarea" v-model="reviewText" placeholder="Write your review"></textarea>
          </div>
        </div>
        <div class="field">
          <div class="control">
            <button class="button is-primary" @click="submitReview">Post Review</button>
          </div>
        </div>
        <h3 class="is-size-3 has-text-centered is-family-cursive has-text-black">Customer Reviews</h3>
        <ul class="review-list">
          <li class="review" v-for="review in reviews" :key="review.id">
            <div class="review-bubble">
              <p>{{ review }}</p>
            </div>
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios';
import ProductBox from '@/components/ProductBox';

export default {
  name: 'HomeView',
  data() {
    return {
      latestProducts: [],
      reviewText: '',
      reviews: []
    };
  },
  components: {
    ProductBox,
  },
  mounted() {
    this.getLatestProducts();
    document.title = 'Home | MoshionApparels';
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit('setIsLoading', true);
      try {
        const response = await axios.get('/api/v1/latest-products/');
        this.latestProducts = response.data;
      } catch (error) {
        console.log(error);
      }
      this.$store.commit('setIsLoading', false);
    },
    submitReview() {
      if (this.reviewText) {
        this.reviews.push(this.reviewText);
        this.reviewText = '';
      }
    },
  },
};
</script>

<style scoped>
.is-family-cursive {
  font-family: 'Your Cool Cursive Font', cursive;
}

.has-text-primary {
  color: #ff7f50; 
}

.has-text-secondary {
  color: #4b0082; 
}

.has-text-black {
  color: #b41de7; /* Black color */
}
.review-list {
  list-style: none;
  padding: 0;
  margin-top: 2rem;
}

.review {
  margin-bottom: 1rem;
}

.review-bubble {
  background-color: #cc5eff;
  color: #000000;
  border-radius: 0.5rem;
  padding: 1rem;
}
</style>
