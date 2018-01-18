<template>
  <div class="puzzle">
    <div v-if="isCorretCode" class="answer">
      <img :src="puzzleCfg.answerImg" />
    </div>
    <div v-else class="question">
      <h2 class="description">{{ this.puzzleCfg.description }}</h2>
      <div class="input-container">
        <div class="bogus-input">
          <div class="input-char" v-for="(inputChar, index) in inputChars" :key="index" :style="{ 'background-color': puzzleCfg.codeBgColors[index] }">
            <span>{{inputChar}}</span>
          </div>
        </div>
        <input type="tel" ref="input" class="real-input" v-model="inputStr" :placeholder="puzzleCfg.inputPlaceholder" :maxlength="this.puzzleCfg.code.length">
      </div>
      <div class="error-msg">
        <span v-show="isCorretCode === false">{{ puzzleCfg.errorMsg }}</span>
      </div>
      <button class="submitBtn" @click="onSubmit">{{ puzzleCfg.submitBtnText }}</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Puzzle',
  data() {
    return {
      puzzleCfg: {},
      isCorretCode: undefined, // 3 status: true, false, undefine(inputting)
      inputChars: [],
      inputStr: '',
    };
  },
  methods: {
    onSubmit() {
      if (this.inputStr.trim() !== this.puzzleCfg.code) {
        this.isCorretCode = false;
        this.$refs.input.focus();
      } else {
        this.isCorretCode = true;
      }
    },
  },
  created() {
    // this.puzzleCfg = JSON.parse(this.$route.query.puzzle);
    this.puzzleCfg = {
      description: '这里是问题',
      submitBtnText: '提交',
      inputPlaceholder: 'CODE',
      code: '1234',
      codeBgColors: ['red', 'green', 'yellow', 'blue'],
      errorMsg: '密码错误',
      answerImg:
        'http://ww2.sinaimg.cn/large/a15b4afegy1fnfedxnog9j208w05hgm4.jpg',
    };
    this.inputChars = this.puzzleCfg.inputPlaceholder.split('');
    this.inputChars.length = this.puzzleCfg.code.length;
  },
  mounted() {
    this.$refs.input.focus();
  },
  watch: {
    inputStr(rawStr) {
      this.isCorretCode = undefined;
      const str = rawStr.trim();
      const len = this.puzzleCfg.code.length;
      for (let i = 0; i < len; i += 1) {
        this.inputChars[i] = str[i];
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" scoped>
code-char-height = 60px;

.puzzle {
  height: 100%;
  text-align: center;
  padding: 100px 30px 0px 30px;
  box-sizing: border-box;
}

.description {
  font-size: 20px;
  margin: 0;
}

.input-container {
  position: relative;
  margin-top: 40px;
}

.real-input {
  opacity: 0;
  text-indent: -999em; // 隐藏input文字
  margin-left: -100%; // 隐藏input光标
  width: 200%; // 保留inputDOM在页面可视区域内
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  color: transparent;
  font-size: 1;
  border: 0;
  outline: none;
}

.bogus-input {
  display: flex;
  justify-content: space-between;

  .input-char {
    text-align: center;
    flex-grow: 1;
    margin: 4px;
    border: 1px solid #fff;
    border-radius: 3px;
    height: code-char-height;
    position: relative;

    &:first-child {
      margin-left: 0px;
    }

    &:last-child {
      margin-right: 0px;
    }

    span {
      font-size: 30px;
      position: absolute;
      height: code-char-height;
      line-height: code-char-height;
      width: 100%;
      top: 0;
      left: 0;
      margin: 0;
      padding: 0;
    }
  }
}

.error-msg {
  margin-top: 10px;
  text-align: left;
  position: relative;
  height: 20px;

  span {
    position: absolute;
  }
}

.submitBtn {
  padding: 10px;
  color: white;
  width: 100%;
  margin-top: 15px;
  background: red;
  border-radius: 3px;
  font-size: 17px;
}

.answer {
  img {
    width: 100%;
  }
}
</style>
