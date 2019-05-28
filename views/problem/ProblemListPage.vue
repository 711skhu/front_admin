<template>
  <div class="container">
    <div class="header">
      <h1>
        연습문제
      </h1>
    </div>
    <div class="problem-list">
      <problem-item
        v-for="problem in problems"
        :key="problem.id"
        :problem="problem"
      />
    </div>
  </div>
</template>

<script>
  import ProblemItemBox from "@/components/problem/ProblemItemBox";
  import ScoreList from "@/admin/components/problem/ScoreList";
  import ProblemItem from "@/admin/components/problem/ProblemItem";
  import Problem from "@/models/problem/Problem";

  let problems = [
    new Problem(1, "1번 문제", 100),
    new Problem(2, "2번 문제", 100),
    new Problem(3, "3번 문제", 100),
    new Problem(4, "4번 문제", 100)
  ];

  export default {
    components: {
      ProblemItem,
      ScoreList,
      ProblemItemBox
    },
    data() {
      return {
        problems: problems,
      }
    }
  }
</script>

<style scoped>
  .header {
    margin-bottom: 6rem;
  }
</style>
