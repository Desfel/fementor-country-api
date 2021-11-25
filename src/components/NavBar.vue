<template>
  <header class="navbar">
    <h1>Where in the world?</h1>

    <a href="#" class="theme-toggler" @click="toggleTheme">
      <svg width="18" height="17" viewBox="0 0 18 17" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path fill-rule="evenodd" clip-rule="evenodd" d="M12.5532 11.815C8.66857 11.815 5.51929 8.92783 5.51929 5.36821C5.51929 4.0253 5.96679 2.78158 6.73143 1.75C3.69036 2.69515 1.5 5.33122 1.5 8.43807C1.5 12.3385 4.94929 15.5 9.20357 15.5C12.5929 15.5 15.4696 13.4932 16.5 10.7045C15.375 11.4048 14.0161 11.815 12.5532 11.815Z" fill="white" stroke="#111517" stroke-width="1.25"/>
      </svg>
      Dark Mode
    </a>
  </header>
</template>

<script>
import { mapState } from 'vuex'

export default {
  data() {
    return {
      isDarkTheme: false
    }
  },
  computed: {
    ...mapState('app', ['networkOnLine', 'appTitle', 'appShortTitle'])
  },
  methods: {
    toggleTheme() {
      this.isDarkTheme = !this.isDarkTheme
      this.$emit('toggleTheme', this.isDarkTheme)
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/theme/variables.scss';

.navbar {
  display: flex;
  width: 100%;
  position: absolute;
  padding: 24px 80px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.0562443);
  background: var(--elementsColor);

  @media (max-width: 767px) {
    padding: 30px 16px;
  }

  h1 {
    font-weight: 800;
    font-size: 24px;
    line-height: 33px;

    @media (max-width: 767px) {
      font-size: 14px;
      line-height: 20px;
    }
  }

  .theme-toggler {
    display: flex;
    align-items: center;
    margin-left: auto;
    font-weight: 600;
    font-size: 16px;
    line-height: 22px;

    @media (max-width: 767px) {
      font-size: 12px;
      line-height: 16px;
    }

    svg {
      margin-right: 10px;

      path {
        stroke: var(--textColor);
      }
    }
  }
}
</style>
