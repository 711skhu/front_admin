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
          <div class="header">Action Forbidden</div>
          <p>You can only sign up for an account once with a given e-mail address.</p>
        </div>
        <div
          class="ui submit button"
          v-on:click="addNewLecture"
        >강의 추가</div>
      </div>
    </form>
    <pre>
      {{ $data }}
    </pre>
  </div>
</template>

<script>
  //import axios from 'axios'

  export default {
    props: ['lectures'],
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
      addNewLecture: function () {
        this.lectures.push({
          title: this.newLectureName,
          professor: this.professorName,
          open: false
        })
        this.newLectureName = ''
      }
    },
    computed: {
      isError() {
        if(this.newLectureName.length > 0) {
          let newLecture = this.newLectureName;
          this.lectures.forEach(function (item) {
            if(item.title === newLecture){
              console.log(item.title + 'vs' + newLecture)
              return true;
            }
          })
        }
        return false;
      },
      isSuccess() {
        if(this.newLectureName.length > 0) {
          let newLecture = this.newLectureName;
          this.lectures.forEach(function (item) {
            if (item.title === newLecture) {
              console.log(item.title === newLecture)
              return false;
            }
            return true;
          })
        }
      }
    }
  }
</script>

<style scoped>
  .addLecture {
    margin: 0 15% 3% 15%;
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
</style>
