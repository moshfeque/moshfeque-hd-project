<template>
  <div id="wrapper">
    <nav class="navbar is-dark" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <router-link to="/" class="navbar-item"><p class="is-family-cursive">Mosh Mart</p></router-link>

        <a class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbar-menu" @click="showMobileMenu = !showMobileMenu">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div class="navbar-menu" id="navbar-menu" v-bind:class="{'is-active': showMobileMenu }">
        <div class="navbar-start">
          <div class="navbar-item">
            <form method="get" action="/search">
              <div class="field has-addons">
                <div class="control">
                  <input type="text" class="input" placeholder="Type your search here" name="query">
                </div>

                <div class="control">
                  <button class="button is-success">
                      <span class="icon">
                      <i class="fas fa-search"></i>
                      </span>
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>

        <div class="navbar-end">
          <router-link to="/men" class="navbar-item is-family-cursive">Men</router-link>
          <router-link to="/women" class="navbar-item is-family-cursive">Women</router-link>

          <div class="navbar-item">
            <div class="buttons">
              <template v-if="$store.state.isAuthenticated">
                <router-link to="/my-account" class="button is-light">My account</router-link>
              </template>

              <template v-else>
                <router-link to="/log-in" class="button is-light">Log in</router-link>
              </template>

              <router-link to="/cart" class="button is-success">
                <span class="icon"><i class="fas fa-shopping-cart"></i></span>
                <span>Cart ({{ cartTotalLength }})</span>
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <div class="is-loading-bar has-text-centered" v-bind:class="{'is-loading': $store.state.isLoading }">
      <div class="lds-dual-ring"></div>
    </div>

    <section class="section">
      <router-view/>
    </section>

    <footer class="footer">
      <p class="has-text-centered"><p is-family-cursive></p>My name is Moshfeque E Jahan (103803710) and this is my HD project for COS30043!</p>
      <p class="has-text-centered">Have fun shopping!</p>
    </footer>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      showMobileMenu: false,
      cart: {
        items: []
      }
    }
  },
  beforeCreate() {
    this.$store.commit('initializeStore')
    const token = this.$store.state.token
    if (token) {
        axios.defaults.headers.common['Authorization'] = "Token " + token
    } else {
        axios.defaults.headers.common['Authorization'] = ""
    }
  },
  mounted() {
    this.cart = this.$store.state.cart
  },
  computed: {
      cartTotalLength() {
          let totalLength = 0
          for (let i = 0; i < this.cart.items.length; i++) {
              totalLength += this.cart.items[i].quantity
          }
          return totalLength
      }
  }
}
</script>

<style lang="scss">
@import '../node_modules/bulma/css/bulma.min.css'; // Import Bulma CSS framework

/* Custom styles */

/* Navbar */
.navbar.is-dark {
  background-color: #f70000;
  color: #000;
}

.navbar-item {
  font-size: 1.2rem;
}

.navbar-item:hover {
  background-color: #dde5ed;
  color: #000;
}

.navbar-burger {
  color: #000;
}

/* Loading bar */
.is-loading-bar {
  background-color: #ffffff22;
}

.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
  animation: lds-dual-ring 1.2s linear infinite;
}

@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/* Footer */
.footer {
  background-color: #a6005e;
  color: #fff;
  padding: 1rem;
}

.footer p {
  margin-bottom: 0;
}

/* Media queries */
@media screen and (max-width: 768px) {
  /* Custom styles for mobile devices */
  .navbar-brand .navbar-item,
  .navbar-menu {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  
  .navbar-burger {
    margin-left: auto;
  }
  
  .navbar-menu {
    flex-basis: 100%;
    margin-top: 10px;
  }
  
  .navbar-start,
  .navbar-end {
    flex-direction: column;
  }
  
  .navbar-item {
    margin-bottom: 10px;
  }
}

.navbar-brand {
  font-family: 'Your Cool Cursive Font', cursive;
  font-size: 2rem;
  color: #000;
}

.navbar-brand:hover {
  color: #f1c40f;
}

.navbar-item {
  font-family: 'Your Cool Cursive Font', cursive;
  font-weight: bold;
  font-size: 1.2rem;
  text-transform: uppercase;
  color: #af5f5f19;
}

.navbar-item:hover {
  color: #f1c40f;
}

body {
  background-color: #ef890d;
  color: #fff;
}
</style>
