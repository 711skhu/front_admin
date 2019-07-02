<template>
  <div>
    <v-textarea
      :value="content"
      @input="(value) => $emit('change', value)"
      label="문제 내용"
      placeholder="여기에 문제 내용을 적어주세요."
      auto-grow
    />
    <v-layout class="markdown-view">
      <markdown-view
        :value="content || '여기에 샘플이 출력됩니다.'"
      ></markdown-view>
    </v-layout>
  </div>
</template>

<script>
  import MarkdownView from "@/components/MarkdownView";

  export default {
    model: {
      prop: 'content',
      event: 'change'
    },
    components: {
      MarkdownView
    },
    props: {
      content: {
        type: String,
        description: "수정 혹은 추가할 문제의 content"
      }
    }
  }
</script>

<style scoped>
  .markdown-view {
    min-height: 200px;
  }
</style>
