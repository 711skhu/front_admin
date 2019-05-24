<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="scores"
      class="elevation-1"
    >
      <template v-slot:items="props">
        <td>{{ props.item.name }}</td>
        <td class="text-xs-right">{{ props.item.score }}</td>
      </template>
    </v-data-table>
  </div>
</template>

<script>
  export default {
    props: {
      maxScore: {
        type: Number,
        required: true,
        description: "만점 성적"
      },
      scores: {
        type: Array,
        required: true,
        description: "성적 목록 type StudentScore"
      }
    },
    data() {
      return {
        headers: [
          {text: '이름', value: 'name'},
          {text: `점수 (만점: ${this.maxScore})`, value: 'score'},
        ]
      }
    }
  }
</script>

<style scoped>

</style>
