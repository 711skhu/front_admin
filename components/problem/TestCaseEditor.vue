<template>
  <v-layout row align-center>
    <v-flex xs5>
      <v-textarea
        rows="1"
        label="input"
        :value="testCase.input"
        @input="value => inputChange(value)"
        placeholder="여기에 입력을 적어주세요."
        auto-grow
        :rules="[rules.required]"
      ></v-textarea>
    </v-flex>
    <v-icon class="mx-2">
      arrow_forward
    </v-icon>
    <v-flex xs5>
      <v-textarea
        rows="1"
        label="output"
        :value="testCase.output"
        @input="value => outputChange(value)"
        placeholder="여기에 출력을 적어주세요."
        auto-grow
        :rules="[rules.required]"
      ></v-textarea>
    </v-flex>
  </v-layout>
</template>

<script>
  import TestCase from "@/models/problem/TestCase";
  import Editor from '@/admin/components/problem/Editor'

  export default {
    model: {
      prop: 'testCase',
      event: 'input'
    },
    mixins: [
      Editor
    ],
    props: {
      testCase: {
        type: TestCase,
        description: '수정 혹은 추가될 테스트 케이스'
      }
    },
    data() {
      return {
        rules: {
          required: v => !!v || '필수 입력입니다.'
        },
      }
    },
    methods: {
      inputChange(input) {
        this.notifyUpdate('input', this.testCase, test => {
          test.input = input;
        })
      },
      outputChange(output) {
        this.notifyUpdate('input', this.testCase, test => {
          test.output = output
        })
      },
    }
  }
</script>

<style scoped>
</style>
