<template>
  <div>
    <header-menu></header-menu>
    <h1 class="title">강의 목록</h1>
    <lecture-list :lectures="lectures"></lecture-list>
    <lecture-adder
      :lectures="lectures"
      @addLecture="addNewLecture"
    ></lecture-adder>
  </div>
</template>

<script>
  import HeaderMenu from '@/components/header/HeaderMenu'
  import LectureList from '@/admin/views/lectureList/LectureList'
  import LectureAdder from '@/admin/views/lectureList/LectureAdder'
  import axios from 'axios'

  export default {
    components: {
      HeaderMenu,
      LectureList,
      LectureAdder
    },
    data() {
      return {
        lectures: []
      }
    },
    methods: {
      addNewLecture: function (lectureName) {
        if (lectureName.length > 0) {
          this.lectures.push({
            title: lectureName,
            professor: '이승진',
            open: false
          })
        } else {
          alert('강의명을 입력해주세요.');
        }
      }
    },
    created() {
      axios.get('http://localhost:8080/server/lectures_professor.json')
        .then((response) => {
          this.lectures = response.data;
          console.log(this.lectures);
        });
    }
  }
</script>

<style scoped>
  h1 {
    margin: 0;
  }
  .title {
    padding: 2% 15% 2% 16%;
    font-size: 25px;
    font-weight: 800;
  }
</style>
