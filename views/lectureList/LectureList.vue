<template>
  <div class="ui middle aligned divided list">
    <div class="item" v-for="lecture in lectures">
      <shadow-box>
        <lecture-item v-bind:lecture="lecture"></lecture-item>
        <v-switch v-model="lecture.open"></v-switch>
      </shadow-box>
    </div>
  </div>
</template>

<script>
  import LectureItem from "@/components/lecture/LectureItem"
  import ShadowBox from "@/components/lecture/ShadowBox"

  export default {
    components: {
      LectureItem,
      ShadowBox
    },
    props: {
      lectures: {
        type: Array,
        required: true,
        description: "lecture list"
      }
    }
  }
</script>
<style>
  .ui.list > .item {
    padding: 0;
  }
  .item:hover .content {
    color: #0078FF;
  }

  .margin-left-150 {
    margin-left: 150px
  }

  .margin-right-150 {
    margin-right: 150px
  }

  .right {
    float: right;
  }

  .ui.list {
    margin: 1em 15%;
    border-top: 0.0625rem solid rgba(50,50,124,0.12);
  }

  .v-input--selection-controls {
    float: right;
    margin-top: -60px;
  }
</style>
