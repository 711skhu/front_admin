<template>
  <div class="problem-page">
    <div class="problem-header">
      <problem-menu
        class="problem-header__problem-menu"
        :problem-details="problemDetails"
        v-model="currentProblem"
      />
      <div class="setting">
        <group-button
          class="test"
          :options="themes"
          v-model="theme"
        />
        <dropdown
          :options="languages"
          v-model="language"
        />
      </div>
    </div>

    <div class="problem-body problem-page__body">
      <div class="problem-body__markdown">
        <markdown-editor
          v-model="currentProblem.content"
          :theme="theme"
        />
      </div>
      <div class="problem-body__gutter"></div>
      <div class="problem-body__code coding">
        <h2 class="coding__file-name"> {{ language.file }} </h2>
        <code-editor
          :theme="theme"
          :lang="language"
          v-model="code"
        />
        <div class="coding__result">
          실행 결과가 표시됩니다.
        </div>
      </div>
    </div>

    <div class="problem-footer">
      <sui-button>질문하기 ({{ "count" }})</sui-button>
      <sui-button>테스트 케이스 추가하기 +</sui-button>
    </div>
  </div>
</template>

<script>
  import MarkdownEditor from "@/components/problem/MarkdownEditor";
  import CodeEditor from "@/components/problem/CodeEditor";
  import ProblemMenu from "@/components/problem/ProblemMenu";

  import ProblemDetail from "@/models/problem/ProblemDetail";
  import GroupButton from "@/components/problem/GroupButton";
  import Dropdown from "@/components/problem/Dropdown";
  import Theme from "@/models/problem/Theme";
  import Language from "@/models/problem/Language";

  export default {
    components: {
      Dropdown,
      GroupButton,
      ProblemMenu,
      MarkdownEditor,
      CodeEditor
    },
    data() {
      let problemDetails = [
        new ProblemDetail(1, "1번 문제", "# 이 문제를 풀어보시오.", 0),
        new ProblemDetail(2, "2번 문제", "## 이 문제를 풀어보시오.", 1),
        new ProblemDetail(3, "3번 문제", "### 이 문제를 풀어보시오.", 2)
      ];
      return {
        problemDetails: problemDetails,
        currentProblem: problemDetails[0],
        themes: [Theme.LIGHT, Theme.DARK],
        theme: Theme.DARK,
        languages: [Language.JAVA, Language.JAVASCRIPT],
        language: Language.JAVA,
        code: "let test = function () {\n\n}"
      }
    }
  }
</script>

<style scoped>
  .problem-page {
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  .problem-body {
    display: flex;
    flex-direction: row;
    border-top: #55565b solid 0.1rem;
    border-bottom: #55565b solid 0.1rem;
  }

  .problem-page__body {
    flex: 1;
  }

  .problem-body__markdown {
    flex: 1 1 50%;
  }

  .problem-body__code {
    flex: 1 1 50%;
  }

  .problem-body__gutter {
    width: 0.1rem;
    height: 100%;

    flex: 1 1;

    background-color: #55565b;
  }

  .problem-header {
    display: flex;
    justify-content: space-between;
  }

  .problem-header__problem-menu {
  }

  .setting {
    display: flex;

    margin-top: 0.35rem;
    margin-bottom: 0.35rem;
  }

  .setting > * {
    margin-right: 0.5rem;
  }

  .problem-footer {
    margin: 0.5rem;
  }

  .coding {
    display: flex;
    flex-direction: column;
  }

  .coding__file-name {
    background-color: #2c2e38;
    color: ghostwhite;

    padding: 0.8rem;
    margin: 0;
  }

  .coding__result {
    width: 30%;
    height: 30%;
  }

  .problem-header__problem-list {

  }

  .problem-header__theme-button {

  }
</style>
