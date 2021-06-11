<template>
  <div id="app">
    <Header
      msg="Welcome to Your Vue.js App"
      :totalCount="this.totalCount"
      :correctCount="this.correctCount"
    />
    <b-container class="bv-example-row">
      <b-row>
        <Content
          v-if="questionlist.length"
          :cQuestion="questionlist[index]"
          :next="next"
          :increment="increment"
        />
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Content from "./components/Content.vue";

export default {
  name: "App",
  components: {
    Header,
    Content,
  },
  data() {
    return {
      questionlist: [],
      index: 0,
      correctCount: 0,
      totalCount: 0,
    };
  },
  methods: {
    next: function () {
      if (this.index < 9) this.index++;
    },
    increment(is_correct) {
      console.log("yes")
      if (is_correct) {
        this.correctCount++;
      }
      this.totalCount++;
    },
  }
  ,
  mounted: function () {
    fetch("https://opentdb.com/api.php?amount=10&category=11", {
      method: "get",
    })
      .then((response) => {
        return response.json();
      })
      .then((result) => {
        this.questionlist = result.results;
        console.log(result.results);
      });
  },
};
</script>


