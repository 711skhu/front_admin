<template>
  <div class="container">
    <div class="header">
      <h1>
        {{ lectureTitle }}
      </h1>
      <div class="buttons">
        <v-tooltip
          v-for="button in buttons"
          :key="button.name"
          bottom>
          <template v-slot:activator="{ on }">
            <v-btn
              @click="button.onClick"
              v-show="button.group === state"
              v-on="on"
              flat icon color="indigo">
              <v-icon>{{button.icon}}</v-icon>
            </v-btn>
          </template>
          <span>{{button.name}}</span>
        </v-tooltip>
      </div>
    </div>
    <div class="content">
      <markdown-view
        class="content__markdown-view"
        v-show="state !== 'edit'"
        :value="descriptionView"></markdown-view>
      <textarea
        class="content__textarea"
        v-show="state === 'edit'"
        v-model="editText"
      />
    </div>
  </div>
</template>

<script>
  import MarkdownView from '@/components/MarkdownView'

  let description = '#### 자바 언어의 기본 문법과 객체 지향 프로그래밍 개념을 단계별로 정립합니다.';

  export default {
    components: {
      MarkdownView,
    },
    data() {
      return {
        buttons: [
          {
            name: '수정',
            icon: 'edit',
            group: 'view',
            onClick: () => this.changeState('edit', () => this.editText = this.description)
          },
          {
            name: '미리보기',
            icon: 'video_label',
            group: 'edit',
            onClick: () => this.changeState('preview', () => this.descriptionView = this.editText)
          },
          {
            name: '저장',
            icon: 'save',
            group: 'edit',
            onClick: () => this.changeState('view', () => {
              this.description = this.editText;
              this.descriptionView = this.description;
            })
          },
          {
            name: '수정 취소',
            icon: 'cancel',
            group: 'edit',
            onClick: () => this.changeState('view', () => this.descriptionView = this.description)
          },
          {
            name: '미리보기 취소',
            icon: 'cancel',
            group: 'preview',
            onClick: () => this.changeState('edit')
          },
        ],
        state: 'view',
        lectureTitle: 'Java Programming',
        description: description,
        editText: description,
        descriptionView: description,
      }
    },
    methods: {
      changeState(targetState, changeText) {
        this.state = targetState;
        if (changeText) {
          changeText();
        }
      },
    }
  };
</script>

<style scoped>
  .container {
    height: 100%;
    background-color: #F3E5F5;
    display: flex;
    flex-direction: column;
  }

  .header {
    display: flex;
    justify-content: space-between;
    color: #7B1FA2;
  }

  .buttons {
    display: flex;
  }

  .content {
    height: 100%;
    display: flex;
    flex-direction: column;
  }

  .content__markdown-view {
    height: 100%;
    overflow: auto;
  }

  .content__textarea {
    height: 100%;
    box-shadow: 0 0 0.7rem rgba(33, 33, 33, .2);
    background-color: whitesmoke;
  }
</style>
