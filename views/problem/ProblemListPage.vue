<template>
  <div class="container">
    <div class="header">
      <h1>
        연습문제
      </h1>
    </div>
    <div class="problem-list">
      <problem-item
        v-for="(problem, index) in problems"
        :key="problem.id"
        :problem="problem"
        @editClick="editProblem(problem, index)"
      />
    </div>
    <v-layout justify-center class="mt-3">
      <v-btn fab dark color="indigo" @click="editProblem()">
        <v-icon dark>add</v-icon>
      </v-btn>
    </v-layout>
    <div class="buttons">
      <v-dialog v-model="dialog" fullscreen hide-overlay transition="dialog-bottom-transition">
        <problem-edit-page
          v-model="editingProblem"
          @cancel="dialog = false"
          @save="saveProblem"
        ></problem-edit-page>
      </v-dialog>
    </div>
  </div>
</template>

<script>
  import ProblemItemBox from "@/components/problem/ProblemItemBox";
  import ScoreList from "@/admin/components/problem/ScoreList";
  import ProblemItem from "@/admin/components/problem/ProblemItem";
  import ProblemEditPage from "@/admin/views/problem/ProblemEditPage";
  import Problem from "@/models/problem/Problem";
  import * as _ from "lodash";

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
      ProblemItemBox,
      ProblemEditPage,
    },
    data() {
      return {
        editingProblem: new Problem(),
        editingIndex: 0,
        problems: problems,
        dialog: false,
      }
    },
    methods: {
      editProblem(problem, index) {
        this.editingProblem = problem ? _.cloneDeep(problem) : new Problem();
        this.editingIndex = index || this.problems.length;
        this.dialog = true;
      },
      saveProblem() {
        this.problems[this.editingIndex] = this.editingProblem;
        this.dialog = false;
      }
    }
  }
</script>

<style scoped>
  .header {
    margin-bottom: 6rem;
  }

  .buttons {
    display: flex;
    justify-content: center;
    padding: 1rem;
  }
</style>
