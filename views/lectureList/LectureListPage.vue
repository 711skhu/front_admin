<template>
  <div>
    <header-menu></header-menu>
    <h1 class="title">강의 목록</h1>
    <lecture-list></lecture-list>
    <lecture-add></lecture-add>
  </div>
</template>

<script>
  import HeaderMenu from '@/components/header/HeaderMenu'
  import LectureList from '@/admin/views/lectureList/LectureList'
  import LectureAdd from '@/admin/views/lectureList/LectureAdd'

  export default {
    components: {
      HeaderMenu,
      LectureList,
      LectureAdd
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
