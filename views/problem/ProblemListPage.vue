<template xmlns:v-slot="http://www.w3.org/1999/XSL/Transform">
  <div class="container">
    <div class="header">
      <h1>
        연습문제
      </h1>
    </div>
    <div class="problem-list">
      <v-expansion-panel
        v-for="problem in problems"
        :key="problem.title"
        popout
      >
        <v-expansion-panel-content>
          <template v-slot:header>
            <problem-item-box class="problem-box">
              <h3>{{problem.title}}</h3>
            </problem-item-box>
          </template>
          <score-list
            :max-score="problem.maxScore"
          />
        </v-expansion-panel-content>
      </v-expansion-panel>
    </div>
  </div>
</template>

<script>
  import ProblemItemBox from "@/components/problem/ProblemItemBox";
  import Problem from "@/models/problem/Problem";
  import ScoreList from "@/components/problem/ScoreList";

  let problems = [
    new Problem(1, "1번 문제", 10),
    new Problem(2, "2번 문제", 12),
    new Problem(3, "3번 문제", 15),
    new Problem(4, "4번 문제", 20)
  ];

  export default {
    components: {
      ScoreList,
      ProblemItemBox
    },
    data() {
      return {
        problems: problems
      }
    }
  }
</script>

<style scoped>
  .header {
    margin-bottom: 6rem;
  }

  .problem-box {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }
</style>
