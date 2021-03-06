<template>
  <div class="mainContent">
    <div class="section__header">
      <div class="sections__container--title" v-if="title">
        <svg width="20" height="20" class="section--icon">
            <use xmlns:xlink="http://www.w3.org/1999/xlink" v-bind:xlink:href="icon"></use>
        </svg>
        <h4 class="mt0 mb0 text--strong">{{title}}</h4>
      </div>
      <a class="close" aria-label="Close button" href="#" @click="close()" title="Close">×</a>
      <slot name="header"></slot>
      <p class="sections__container--description text--center text--small px2 py1" v-if="desc">{{desc}}</p>
    </div>
    <template v-if="loading">
      <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="40" height="40" viewBox="0 0 40 40">
        <path opacity=".2" d="M20.201,5.169c-8.254,0-14.946,6.692-14.946,14.946c0,8.255,6.692,14.946,14.946,14.946 s14.946-6.691,14.946-14.946C35.146,11.861,28.455,5.169,20.201,5.169z M20.201,31.749c-6.425,0-11.634-5.208-11.634-11.634 c0-6.425,5.209-11.634,11.634-11.634c6.425,0,11.633,5.209,11.633,11.634C31.834,26.541,26.626,31.749,20.201,31.749z"/>
        <path d="M26.013,10.047l1.654-2.866c-2.198-1.272-4.743-2.012-7.466-2.012h0v3.312h0 C22.32,8.481,24.301,9.057,26.013,10.047z">
          <animateTransform attributeType="xml"
            attributeName="transform"
            type="rotate"
            from="0 20 20"
            to="360 20 20"
            dur="0.7s"
            repeatCount="indefinite"/>
        </path>
      </svg>
    </template>
    <template v-else>
      <template v-if="hasData">
        <div class="scrollable _word--break" :class="getContainerHeight()">
          <slot></slot>
        </div>
        <slot name="filters"></slot>
      </template>
      <template v-else>
        <p v-if="error" class="alert--danger">ERROR: {{error}}</p>
        <p v-else class="alert--warning">{{noDataMsg}}</p>
      </template>
    </template>
  </div>
</template>
<script>
import SectionContainer from "./../../sections/section-container";

export default {
  mixins: [SectionContainer],
  computed: {
    desc() {
      return this.$parent.desc;
    }
  },
  methods: {
    getContainerHeight() {
      switch(this.currentPanel){
        case "performance":
          return 'scrollable--performance';
        case "accessibility":
          return 'scrollable--accessibility';
        case "html5":
          return 'scrollable--html';
        default:
          return '_h--350';
      }
    },
    close() {
      EventBus.$emit("changePanel", "tech");
    }
  }
};
</script>
<style lang="scss" scoped>

// Sections
.section {

  &__header {
    position: relative;
    background-image: linear-gradient(to bottom, transparent, rgba(0,0,0,.3));
    color: #fff;
    margin: -1rem -1rem 0 -1rem;
    padding-bottom: 1rem;

    &--title {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-bottom: 2rem;
    }
  }
}

.sections__container--title {
  padding-left: 1.2rem;
}

._word--break {
  word-break: break-word;
}

._h {
  &--350 {
    height: 350px !important;
  }
}


// Sections - scrollable area
.scrollable {
  width: 100%;
  height: 345px;
  overflow-y: auto;
  margin-top: 1rem;

  &__container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 1rem;
  }

  &--issue {
    padding: 1rem;

    &:not(:last-of-type) {
      margin-bottom: 1rem;
    }
  }

  &--performance {
    height: calc(100vh - 300px) !important;
    margin-top: 10px;
  }

  &--accessibility {
    height: calc(100vh - 300px) !important;
    margin-top: 10px;
  }

  &--html {
    height: calc(100vh - 275px) !important;
    margin-top: 10px;
  }
}

// Close button
.close {
  position: absolute;
  top: 0;
  right: 0;
  padding: .5rem 1.2rem 1rem 1.2rem;
  font-size: 1.5rem;
  font-weight: 700;
  line-height: 1;
  color: #fff;
  opacity: .5;
  text-decoration: none;

  &:hover {
    opacity: 1;
  }
}

// Empty states
.state {
  display: flex;
  height: 100%;
  align-items: center;
  justify-content: center;

  &--sparks {
    animation: sparks .5s linear;
  }

  &--success {
    stroke-dasharray: 1000;
    stroke-dashoffset: 0;
    animation: success 1s linear;
  }
}

@keyframes sparks {
  from {
    transform: scale(0);
  }

  to {
    transform: scale(1);
  }
}

@keyframes success {
  from {
    stroke-dashoffset: 1000;
  }
  to {
    stroke-dashoffset: 0;
  }
}
</style>
