<template>
  <v-card>
    <v-toolbar dark color="primary">
      <v-toolbar-title>Edit Page</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-btn dark flat @click="saveProblem">저장</v-btn>
        <v-btn icon dark @click="$emit('cancel')">
          <v-icon>close</v-icon>
        </v-btn>
      </v-toolbar-items>
    </v-toolbar>
    <v-container>
      <v-text-field
        label="문제 제목"
        :value="problem.title"
        @input="titleChange"
        placeholder="문제 제목을 작성해주세요."
      ></v-text-field>

      <h3>문제 상세</h3>

      <template v-for="(problemDetail, index) in problemDetails">
        <v-layout>
          <problem-detail-item
            :key="index"
            :detail="problemDetail"
            @edit="editDetail(index)"
          ></problem-detail-item>
          <v-btn icon flat @click="deleteDetail(index)">
            <v-icon>close</v-icon>
          </v-btn>
        </v-layout>
        <v-divider
          v-if="index + 1 !== problemDetails.length"
        ></v-divider>
      </template>
      <v-layout justify-center>
        <v-btn @click="editDetail(problemDetails.length)">
          문제 상세 추가
        </v-btn>
      </v-layout>
      <v-dialog v-model="dialog" persistent>
        <v-card>
          <v-layout justify-end>
            <v-btn icon flat @click="dialog = false">
              <v-icon large>close</v-icon>
            </v-btn>
          </v-layout>
          <problem-detail-editor
            v-model="editingDetail"
            @save="saveProblemDetail"
          ></problem-detail-editor>
        </v-card>
      </v-dialog>
      <v-alert
        :value="!isPossibleSave"
        color="error"
        icon="warning"
        outline
      >
        문제 제목은 필수입니다.
      </v-alert>
    </v-container>
  </v-card>
</template>

<script>
  import Problem from "@/models/problem/Problem";
  import ProblemDetailItem from "@/admin/components/problem/ProblemDetailItem";
  import ProblemDetailEditor from "@/admin/components/problem/ProblemDetailEditor";
  import ProblemDetail from "@/models/problem/ProblemDetail";
  import * as _ from "lodash";
  import Editor from "@/admin/components/problem/Editor";

  export default {
    model: {
      prop: 'problem',
      event: 'input'
    },
    mixins: [Editor],
    components: {
      ProblemDetailEditor,
      ProblemDetailItem
    },
    props: {
      problem: {
        type: Problem,
        description: "화면에 보여질 Problem 객체"
      }
    },
    data() {
      return {
        dialog: false,
        rules: {
          required: v => !!v || 'This field is required'
        },
        localTitle: this.problem.title,
        problemDetails: [],
        editingDetail: new ProblemDetail(),
        editingDetailIndex: 0,
      }
    },
    methods: {
      saveProblemDetail() {
        this.problemDetails[this.editingDetailIndex] = this.editingDetail;
        this.dialog = false;
      },
      editDetail(targetIndex) {
        this.editingDetailIndex = targetIndex;
        this.editingDetail = _.cloneDeep(this.problemDetails[targetIndex] || new ProblemDetail());
        this.dialog = true;
      },
      saveProblem() {
        if (this.isPossibleSave) {
          this.$emit('save');
        }
      },
      titleChange(value) {
        this.notifyUpdate('input', this.problem, p => {
          p.title = value;
        })
      },
      deleteDetail(index) {
        this.problemDetails.splice(index, 1);
      }
    },
    computed: {
      isPossibleSave() {
        return !!(this.problem.title.trim().length)
      },
    },
    beforeUpdate() {
      // get problem details from server
    }
  }
</script>

<style scoped>

</style>
