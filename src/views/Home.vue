<template>
  <div class="page-wrapper">
    <section class="form-section">
        <form class="country-search" action="#" method="POST">
          <div class="input-cell">
            <svg width="18" height="18" viewBox="-1 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path fill-rule="evenodd" clip-rule="evenodd" d="M12.5 11H11.7L11.4 10.7C12.4 9.6 13 8.1 13 6.5C13 2.9 10.1 0 6.5 0C2.9 0 0 2.9 0 6.5C0 10.1 2.9 13 6.5 13C8.1 13 9.6 12.4 10.7 11.4L11 11.7V12.5L16 17.5L17.5 16L12.5 11ZM6.5 11C4 11 2 9 2 6.5C2 4 4 2 6.5 2C9 2 11 4 11 6.5C11 9 9 11 6.5 11Z" fill="#848484"/>
            </svg>

            <input name="countryName" placeholder="Search for a country…" />
          </div>
        </form>

        <div class="region-select-wrapper">
          <div class="region-main input-cell" @click="openDropdown">
            <p>Filter by Region</p>
            <svg :class="{'is-open':dropdownOpenState}" width="12" height="12" viewBox="0 0 12 12" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path fill-rule="evenodd" clip-rule="evenodd" d="M9.45 3.45L6 6.9L2.55 3.45L1.5 4.5L6 9L10.5 4.5L9.45 3.45Z" fill="black"/>
            </svg>
          </div>

          <div class="regions-dropdown input-cell" :class="{'is-open':dropdownOpenState}">
            <p>Canada</p>
            <p>Japan</p>
            <p>Switzerland</p>
          </div>
        </div>
    </section>

    <section class="countries-wrapper">
      <div class="country-card" v-for="n in 8" v-bind:key="n">
        <img src="@/assets/img/temp-flag.jpg" alt="Germany" />

        <div class="country-content">
          <h2>Germany</h2>
          <p><span>Population:</span> 81,770,900</p>
          <p><span>Region:</span> Europe</p>
          <p><span>Capital:</span> Berlin</p>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { mapState } from 'vuex'

export default {
  data() {
    return {
      dropdownOpenState: false
    }
  },
  head() {
    return {
      title: {
        inner: 'Home'
      },
      meta: [
        {
          name: 'description',
          content: `${this.appTitle} home page`,
          id: 'desc'
        }
      ]
    }
  },
  methods: {
    openDropdown() {
      this.dropdownOpenState = !this.dropdownOpenState
    }
  },
  computed: mapState('app', ['appTitle'])
}
</script>

<style lang="scss" scoped>
@import '@/theme/variables.scss';

.page-wrapper {
  display: flex;
  flex-direction: column;

  .form-section {
    display: flex;
    align-items: center;
    margin-bottom: 48px;

    @media (max-width: 767px) {
      flex-direction: column;
      align-items: flex-start;
      padding: 0 16px;
      margin-bottom: 32px;
    }
  }

  .input-cell {
    display: flex;
    align-items: center;
    padding: 19px 23px;
    border-radius: 5px;
    box-shadow: 0 2px 9px rgba(0, 0, 0, 0.0532439);
    background: var(--elementsColor);
  }

  input {
    width: 100%;
    border: none;
    outline: none;
    background: var(--elementsColor);
    font-family: $textFont;
    font-style: normal;
    font-weight: normal;
    font-size: 14px;
    line-height: 20px;
    color: var(--textColor);

    @media (max-width: 767px) {
      font-size: 12px;
      line-height: 20px;
    }

    &::placeholder {
      color: var(--inputColor);
    }
  }

  .country-search {
    width: 100%;
    max-width: 450px;

    @media (max-width: 767px) {
      max-width: 100%;
    }

    .input-cell {
      width: 100%;

      svg {
        margin-right: 24px;
        stroke: var(--inputColor);
        transition: none;

        path {
          fill: var(--inputColor);
          transition: none;
        }
      }
    }
  }

  .region-select-wrapper {
    position: relative;
    margin-left: auto;

    @media (max-width: 767px) {
      margin-top: 40px;
      margin-left: 0;
    }

    .region-main {
      display: flex;
      align-items: center;
      font-weight: normal;
      font-size: 14px;
      line-height: 20px;
      color: var(--textColor);
      cursor: pointer;

      @media (max-width: 767px) {
        font-size: 12px;
        line-height: 20px;
      }

      svg {
        margin-left: 47px;
        stroke: var(--textColor);
        transition: transform .5s ease-in-out;

        &.is-open {
          transform: rotate(-180deg);
        }

        path {
          fill: var(--textColor);
          transition: none;
        }
      }
    }

    .regions-dropdown {
      position: absolute;
      width: 100%;
      top: 45px;

      display: flex;
      flex-direction: column;
      align-items: flex-start;
      padding: 16px 24px;
      opacity: 0;
      pointer-events: none;

      &.is-open {
        opacity: 1;
        pointer-events: auto;
        top: 65px;
      }

      p {
        font-size: 14px;
        line-height: 20px;
        cursor: pointer;

        @media (max-width: 767px) {
          font-size: 12px;
          line-height: 16px;
        }

        &:hover {
          opacity: .7;
        }

        &:not(:last-child) {
          margin-bottom: 8px;
        }
      }
    }
  }

  .countries-wrapper {
    display: flex;
    flex-wrap: wrap;
    margin-bottom: -30px;

    @media (max-width: 767px) {
      padding: 0 55px;
    }

    .country-card {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      width: calc((100% - 75px * 3) / 4);
      margin-bottom: 75px;
      background: var(--elementsColor);
      box-shadow: 0 0 7px 2px rgba(0, 0, 0, 0.0294384);
      border-radius: 5px;

      @media (min-width: 1201px) {
        &:not(:nth-child(4n + 4)) {
          margin-right: 75px;
        }
      }

      @media (min-width: 1025px) and (max-width: 1200px) {
        width: calc((100% - 75px * 2) / 3);

        &:not(:nth-child(3n + 3)) {
          margin-right: 75px;
        }
      }

      @media (min-width: 768px) and (max-width: 1024px) {
        margin-bottom: 50px;
        width: calc((100% - 50px) / 2);

        &:not(:nth-child(2n + 2)) {
          margin-right: 50px;
        }
      }

      @media (max-width: 767px) {
        margin-bottom: 50px;
        width: 100%;
      }

      img {
        border-radius: 5px 5px 0 0;
        width: 100%;
      }

      .country-content {
        display: flex;
        flex-direction: column;
        padding: 24px;

        h2 {
          font-weight: 800;
          font-size: 18px;
          line-height: 26px;
          margin-bottom: 16px;
        }

        p {
          font-weight: 300;
          font-size: 14px;
          line-height: 16px;

          &:not(:last-child) {
            margin-bottom: 8px;
          }

          span {
            font-weight: 600;
          }
        }
      }
    }
  }
}
</style>
