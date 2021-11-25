<template>
  <div id="app" :class="{'theme-dark' : isDarkTheme}">
    <nav-bar @toggleTheme="toggleTheme"></nav-bar>
    <div class="main-wrapper">
      <router-view />
    </div>
  </div>
</template>
<script>
import NavBar from '@/components/NavBar'
import { mapState, mapGetters } from 'vuex'

export default {
  data() {
    return {
      isDarkTheme: false
    }
  },
  components: { NavBar },
  computed: {
    ...mapGetters('app', ['newContentAvailable']),
    ...mapState('app', ['showAddToHomeScreenModalForApple', 'refreshingApp'])
  },
  methods: {
    toggleTheme(value) {
      this.isDarkTheme = value
    }
  }
}
</script>

<style lang="scss">
@import '@/theme/variables.scss';
body {
  margin: 0;

  * {
    box-sizing: border-box;
    transition: all .3s ease-in-out;
  }

  img {
    vertical-align: middle;
  }

  p,
  a,
  h1,
  h2,
  a {
    margin: 0;
    font-family: $textFont;
    color: var(--textColor);
    transition: all .3s ease-in-out;
  }

  a {
    text-decoration: none;
  }

  #app {
    font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen,
      Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    font-size: 16px;
    color: #2c3e50;

    .new-content-available-toastr {
      position: absolute;
      bottom: 10px;
      right: 10px;
    }

    .main-wrapper {
      .page-wrapper {
        width: 100%;
        height: 100%;
        padding: 128px 80px 45px;
        min-height: 100vh;
        margin: auto;
        background: var(--bgColor);
        transition: background .3s ease-in-out;

        @media (max-width: 767px) {
          padding: 104px 0 65px;
        }
      }
    }
  }
}
</style>
