<template>
  <problem-item-box class="problem-box">
    <div class="problem-header">

      <v-layout align-baseline>
        <h2>{{problem.title}}</h2>
        <v-btn flat icon @click="$emit('editClick')">
          <v-icon dark>edit</v-icon>
        </v-btn>
      </v-layout>

      <div class="problem-header__buttons">

        <v-hover>
          <v-progress-circular
            style="cursor: pointer"
            slot-scope="{ hover }"
            :rotate="-90"
            :size="80"
            :width="5"
            :value="solvedStudent.length / students.length * 100"
            @click="showScore = !showScore"
          >
            <v-slide-y-transition mode="out-in">
              <div v-if="!hover" key="count">
                {{`${solvedStudent.length}/${students.length}`}}
              </div>
              <div v-else key="icon">
                <v-icon>list</v-icon>
              </div>
            </v-slide-y-transition>
          </v-progress-circular>
        </v-hover>

        <v-btn
          @click="updateStudents"
          flat icon color="green"
        >
          <v-icon>cached</v-icon>
        </v-btn>
      </div>
    </div>

    <div class="problem-content">
      <v-expand-transition>
        <score-list
          class="problem-content__list"
          v-show="showScore"
          :scores="solvedStudent"
          :max-score="problem.maxScore"
        />
      </v-expand-transition>
    </div>
  </problem-item-box>
</template>

<script>
  import ProblemItemBox from "@/components/problem/ProblemItemBox";
  import ScoreList from "@/admin/components/problem/ScoreList";
  import StudentScore from "@/models/problem/StudentScore";
  import Problem from "@/models/problem/Problem";

  export default {
    components: {
      ProblemItemBox,
      ScoreList
    },
    props: {
      problem: {
        type: Problem,
        required: true,
        description: "화면에 보여질 문제 정보"
      }
    },
    data() {
      return {
        showScore: false,
        students: []
      }
    },
    methods: {
      updateStudents() {
        this.students = Array.apply(null, {length: Math.floor(Math.random() * 24) + 24})
          .map(e => this.studentsGenerator());
      },
      studentsGenerator() {
        let name = '학생' + Math.floor(Math.random() * 41) + 1;
        let score = null;

        if (Math.random() < 0.5) {
          score = Math.floor(Math.random() * 100) + 1;
        }

        return new StudentScore(name, score);
      }
    },
    computed: {
      solvedStudent() {
        return this.students.filter(student => student.score);
      },
    },
    mounted() {
      this.updateStudents();
    }
  }
</script>

<style scoped>
  .problem-box {
    display: flex;
    flex-direction: column;
  }

  .problem-header {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .problem-content__list:before {
    content: '';
    display: block;
    margin-top: 1.2rem;
  }
</style>
