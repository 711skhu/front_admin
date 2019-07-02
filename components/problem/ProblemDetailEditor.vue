<template>
  <v-container>
    <v-stepper v-model="currentStep">
      <v-stepper-header>
        <template v-for="(step, index) in steps">
          <v-stepper-step
            :key="`${step}-step`"
            :step="index + 1"
            :complete="currentStep > index + 1"
            editable
            :rules="[() => currentStep > (index + 1) ? rules[index]() : true]"
          >
            {{ step }}
          </v-stepper-step>

          <v-divider
            v-if="index + 1 !== steps.length"
            :key="index"
          ></v-divider>
        </template>
      </v-stepper-header>

      <v-stepper-items>
        <v-stepper-content
          :step="1"
        >
          <v-card class="mb-4">
            <v-text-field
              :value="detail.title"
              @input="value => notify(p => p.title = value)"
              placeholder="문제 제목을 작성해주세요."
            ></v-text-field>
            <problem-detail-content-editor
              :content="detail.content"
              @change="value => notify(p => p.content = value)"
            ></problem-detail-content-editor>
          </v-card>

          <v-btn
            color="primary"
            @click="moveStep(2)"
          >
            다음
          </v-btn>
        </v-stepper-content>

        <v-stepper-content
          :step="2"
        >
          <v-card class="mb-4">
            <v-layout row wrap align-center>
              <v-flex xs12 sm6>
                <v-subheader v-text="'문제에서 사용 가능한 언어'"></v-subheader>
              </v-flex>

              <v-flex xs12 sm6>
                <v-select
                  :value="detail.supportedLanguages"
                  @input="value => notify(p => p.supportedLanguages = value)"
                  :items="languages"
                  label="언어"
                  multiple
                  persistent-hint
                ></v-select>
              </v-flex>
            </v-layout>
          </v-card>

          <v-btn
            color="primary"
            @click="moveStep(3)"
          >
            다음
          </v-btn>

          <v-btn flat @click="moveStep(1)">이전</v-btn>
        </v-stepper-content>

        <v-stepper-content
          :step="3"
        >
          <v-card
            class="mb-4"
          >
            <template
              v-for="(testCase, index) in detail.testCases"
            >
              <v-layout>
                <test-case-editor
                  :key="index"
                  :test-case="detail.testCases[index]"
                  @input="value => notify(p => p.testCases[index] = value)"
                ></test-case-editor>
                <v-btn
                  @click="deleteTestCase(index)"
                  icon flat>
                  <v-icon>close</v-icon>
                </v-btn>
              </v-layout>
              <v-divider
                v-if="index + 1 !== detail.testCases.length "
              ></v-divider>
            </template>
            <v-layout justify-center>
              <v-btn
                small
                outline
                color="primary"
                @click="addTestCase"
              >
                테스트 케이스 추가
              </v-btn>
            </v-layout>
          </v-card>

          <v-btn
            color="primary"
            @click="moveStep(4)"
          >
            다음
          </v-btn>

          <v-btn flat @click="moveStep(2)">이전</v-btn>
        </v-stepper-content>

        <v-stepper-content
          :step="4"
        >
          <v-layout justify-center>
            <v-btn
              color="primary"
              @click="notifySave"
            >
              저장
            </v-btn>
            <v-btn flat @click="moveStep(3)">이전</v-btn>
          </v-layout>
        </v-stepper-content>
      </v-stepper-items>
    </v-stepper>
    <v-alert
      :value="!isPossibleSave"
      color="error"
      icon="warning"
      outline
    >
      미작성된 항목이 있습니다.
    </v-alert>
  </v-container>
</template>

<script>
  import ProblemDetail from "@/models/problem/ProblemDetail";
  import TestCaseEditor from '@/admin/components/problem/TestCaseEditor';
  import TestCase from "@/models/problem/TestCase";
  import Editor from '@/admin/components/problem/Editor';
  import ProblemDetailContentEditor from "@/admin/components/problem/ProblemDetailContentEditor";
  import Language from '@/models/enums/problem/Language';

  const steps = [
    '문제 작성',
    '지원 언어 선택',
    '테스트 케이스 작성',
    '저장'
  ];

  export default {
    model: {
      prop: 'detail',
      event: 'update'
    },
    mixins: [
      Editor
    ],
    components: {
      ProblemDetailContentEditor,
      TestCaseEditor
    },
    props: {
      detail: {
        type: ProblemDetail,
        description: "수정 혹은 작성할 새끼 문제 항목"
      }
    },
    data() {
      return {
        currentStep: 1,
        steps: steps,
        languages: Language.values,
      }
    },
    methods: {
      notify(modification) {
        this.notifyUpdate('update', this.detail, modification)
      },
      addTestCase() {
        this.notify(p => {
          (p.testCases || (p.testCases = [])).push(new TestCase());
        })
      },
      deleteTestCase(index) {
        this.notify(p => {
          p.testCases.splice(index, 1);
        })
      },
      moveStep(n) {
        this.currentStep = n;
      },
      notifySave() {
        if (this.isPossibleSave) {
          this.$emit('save');
          this.moveStep(1);
        } else {
          this.moveErrorStep();
        }
      },
      moveErrorStep() {
        for (let i = 0; i < this.rules.length - 1; i++) {
          if (!this.rules[i]()) {
            this.moveStep(i + 1);
            break;
          }
        }
      }
    },
    computed: {
      rules() {
        let content = () => {
          return this.detail.title && this.detail.content && true;
        };
        let languages = () => this.detail.supportedLanguages.length && true;
        let testCases = () => {
          return this.detail.testCases.length
            && this.detail.testCases
              .filter(testCase =>
                !String(testCase.input).trim().length
                || !String(testCase.output).trim().length
                || false
              ).length === 0
            && true;
        };
        let save = () => content() && languages() && testCases() && true;

        return [
          content,
          languages,
          testCases,
          save
        ]
      },
      isPossibleSave() {
        return this.rules[3]();
      },
    }
  }
</script>

<style scoped>

</style>
