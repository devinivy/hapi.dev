<template>
  <div class="container">
    <TutorialNav
      :language="language"
      @changed="onChangeChild"
    />
    <div class="tutorial-markdown-window">
      <Tutorial :display="getPage" :language="language"/>
    </div>
  </div>
</template>

<script>
import Tutorial from "../../components/tutorials/Tutorial.vue";
import TutorialNav from "../../components/tutorials/TutorialNav.vue";
const page = require("../../static/lib/tutorials/");
export default {
  components: {
    Tutorial,
    TutorialNav
  },
  head() {
    return {
      title:
        "hapi.js - Getting Started"
    };
  },
  data() {
    return {
      language: this.getLanguage
    };
  },
  computed: {
    getPage() {
      // /<ul>([^;]*)<\/ul>/
      return this.$store.getters.loadPage;
    },
    getLanguage() {
      return this.$store.getters.loadLanguage
    }
  },
  methods: {
    onChangeChild(value) {
      this.$store.commit("setLanguage", value)
      this.$store.commit("setPage", page[value].gettingStarted.default);
      window.scrollTo(0, 0);
    }
  },
  created() {
    this.$store.commit("setDisplay", "tutorials");
    this.$store.commit("setLanguage", this.$route.query.lang)
    this.$store.commit("setPage", page.en_US.gettingStarted.default);
  }
};
</script>

<style lang="scss">
@import "../../assets/styles/main.scss";
@import "../../assets/styles/api.scss";

section > ol {
  padding: 20px 20px 13px 20px;
  border: 1px solid $dark-white;
  margin: 0;
  background: $off-white;
}

ol {
  counter-reset: item;
}

.markdown-wrapper ol ol {
  margin: 5px 0 0 20px;
}

.markdown-wrapper ol li {
  margin: 0 0 7px 0;
  display: block;
  list-style-type: none;
  list-style-position: inside;
}

.markdown-wrapper ol li:before {
  content: counters(item, ".") ". "; 
  counter-increment: item;
}
</style>
