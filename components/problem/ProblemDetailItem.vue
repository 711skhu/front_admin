<template>
  <v-container>
    <v-layout>
      <v-layout column>
        <h3>{{ detail.title }}</h3>
        <div>
          <v-chip
            v-for="lang in detail.supportedLanguages"
            :key="lang"
            outline color="primary">
            {{lang}}
          </v-chip>
        </div>
      </v-layout>
      <v-spacer></v-spacer>
      <v-layout justify-end>
        <v-btn outline @click="$emit('edit')">
          수정하기
        </v-btn>
        <v-btn outline>
          댓글보기
        </v-btn>
      </v-layout>
    </v-layout>
  </v-container>
</template>

<script>
  import ProblemDetail from "@/models/problem/ProblemDetail";

  export default {
    props: {
      detail: {
        type: ProblemDetail,
        required: true,
        description: "화면에 보여질 문제 상세"
      }
    }
  }
</script>

<style scoped>

</style>
