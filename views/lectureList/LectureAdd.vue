<template>
  <div class="addLecture">
    <div class="ui horizontal divider">
      강의 추가
    </div>
    <div class="ui form success">
      <div class="field">
        <label>강의명</label>
        <input type="text" placeholder="강의명을 입력해주세요.">
      </div>
      <div class="ui success message">
        <div class="header">강의를 생성할 수 있습니다.</div>
        <p>강의 추가 버튼을 클릭해 강의를 생성하세요.</p>
      </div>
      <div class="ui error message">
        <div class="header">Action Forbidden</div>
        <p>You can only sign up for an account once with a given e-mail address.</p>
      </div>
      <div class="ui submit button">강의 추가</div>
    </div>
  </div>
</template>

<script>

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
</style>
