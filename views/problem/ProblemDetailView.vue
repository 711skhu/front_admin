<template>
  <div class="problem">
    <div class="content">
      <div class="content__markdown">
        <markdown-editor
          :value="markdownString"
          :theme="themes[theme].markdown"
        />
      </div>
      <div class="content__code">
        <code-editor
          :theme="themes[theme].code"
          :mode="mode"
          :init-code="initCode"
        />
      </div>
    </div>
  </div>
</template>

<script>
  import MarkdownEditor from "@/components/problem/MarkdownEditor";
  import CodeEditor from "@/components/problem/CodeEditor";

  export default {
    components: {
      CodeEditor,
      MarkdownEditor
    },
    data() {
      return {
        markdownString: "# header",
        themes: {
          dark: {
            markdown: "dark",
            code: "dracula"
          },
          light: {
            markdown: "light",
            code: "chrome"
          }
        },
        theme: "dark",
        mode: "javascript",
        initCode: "let test = function () {\n\n}"
      }
    }
  }
</script>

<style scoped>
  .problem {
    width: 100%;
    height: 100%;
  }

  .content {
    width: 100%;
    height: 100%;

    display: flex;
    flex-direction: row;
  }

  .content__markdown {
    flex: 1 1 40%;
    border-right: gray solid 0.2rem;
  }

  .content__code {
    flex: 1 1 60%;
  }
</style>
