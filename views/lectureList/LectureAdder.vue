<template>
  <div class="addLecture">
    <form>
      <div class="ui horizontal divider">
        강의 추가
      </div>
      <div
        class="ui form"
        :class="{ success: isSuccess, error: isError}"
      >
        <div class="field">
          <label>강의명</label>
          <input
            type="text"
            v-model="newLectureName"
            placeholder="강의명을 입력해주세요.">
        </div>
        <div
          v-if="isSuccess"
          class="ui success message">
          <div class="header">강의를 생성할 수 있습니다.</div>
          <p>강의 추가 버튼을 클릭해 강의를 생성하세요.</p>
        </div>
        <div
          v-if="isError"
          class="ui error message">
          <div class="header">강의를 생성할 수 없습니다.</div>
          <p>중복되는 강의명을 입력했습니다. 변경해주세요.</p>
        </div>
        <div
          class="ui submit button"
          :class=" { disabled: isError } "
          v-on:click="addLecture"
        >강의 추가</div>
      </div>
    </form>
  </div>
</template>

<script>
  export default {
    props: {
      lectures: {
        type: Array,
        required: true,
        description: "lecture list"
      }
    },
    data() {
      return {
        newLectureName:'',
        professorName: '이승진',
        lectures: this.lectures,
        //isSuccess: false,
        error: false
      }
    },
    methods: {
      addLecture: function () {
        this.$emit('addLecture', this.newLectureName)
      }
    },
    computed: {
      isError() {
        let newLectureTitle = this.newLectureName;
          for(let i = 0; i < this.lectures.length; i++) {
            if(newLectureTitle == this.lectures[i].title)
              return true;
          }
      },
      isSuccess() {
        let newLectureTitle = this.newLectureName;
        if(newLectureTitle.length > 0) {
          for(let i = 0; i < this.lectures.length; i++) {
            if(newLectureTitle == this.lectures[i].title)
              return false;
          }
          return true;
        }
      }
    }
  }
</script>

<style scoped>
  .addLecture {
    margin: 0 15% 5% 15%;
  }
  .success {
    background-color: white !important;
  }
  .ui.success.message {
    background-color: #fcfff5 !important;
  }
  .ui.button {
    float: right;
  }
  .error {
    background-color: white !important;
  }
  .ui.error.message {
    background-color: #fff6f6 !important;
  }
</style>
