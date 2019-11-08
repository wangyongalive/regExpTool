<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <textarea class="input" ref="input" placeholder="输入待匹配的文本" v-model="txt"></textarea>
    <div class="inputRow">
      正则表达式:<input ref="reg" type="text" v-model="reg" placeholder="在此输入正则表达式">
      <input id="modifierI" type="checkbox" value="i" v-model="checkBox"/>
      <label for="modifierI">忽略大小写</label>
      <input id="modifierG" type="checkbox" value="g" v-model="checkBox"/>
      <label for="modifierG">全局匹配</label>
      <input id="modifierM" type="checkbox" value="m" v-model="checkBox"/>
      <label for="modifierM">多行匹配</label>
      <input type="button" id="matchingBtn" value="测试匹配" @click="test"/>
    </div>
    <div class="outputRow">
      匹配结果：
      <div class="resDiv" ref="resDiv"></div>
    </div>
    <div class="inputRow">
      <input ref="reg" type="text" v-model="replaceTxt" placeholder="在此输入要替换的文本">
      <input type="button" id="replaceBtn" value="替换" ref="replace" @click="replace"/>
    </div>
    <div class="outputRow">
      替换结果：
      <div class="resDiv" ref="replaceDiv"></div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        msg: '正则表达式测试工具',
        txt: "",
        reg: "",
        replaceTxt: "",
        checkBox: []
      }
    },
    methods: {
      base(){
        if (!this.txt) {
          alert("输入待匹配的字符串");
          this.$refs.input.focus();
          return;
        }
        if (!this.reg) {
          alert("输入正则表达式");
          this.$refs.reg.focus();
          return;
        }
      },
      test() {
        this.base();
        // 分组 方便后面引用
        let pattern = new RegExp(`(${this.reg})`, this.checkBox.join(""));
        this.$refs.resDiv.innerHTML = pattern.exec(this.txt) ? this.txt.replace(pattern, `<span style="background-color: yellow">$1</span>`) : '(没有匹配)';
      },
      replace() {
        this.base();
        if (!this.replaceTxt) {
          alert("输入想替换的文本");
          this.$refs.replace.focus();
          return;
        }
        // 分组 方便后面引用
        let pattern = new RegExp(`(${this.reg})`, this.checkBox.join(""));
        this.$refs.replaceDiv.innerHTML = this.txt.replace(pattern,`<span style="color: red">${this.replaceTxt}</span>`)
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1 {
    font-weight: normal;
  }

  .hello {
    width: 650px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .input {
    box-sizing: border-box;
    height: 100px;
    width: 100%;
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 5px;
    resize: none;
  }

  .inputRow {
    width: 100%; /*设置宽度*/
    margin-top: 10px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
  }

  .outputRow {
    width: 100%; /*设置宽度*/
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: space-between;
  }

  /*使用flex之后div无法独占一行*/
  .resDiv {
    background-color: #eee;
    box-sizing: border-box;
    border: 1px solid #ccc;
    width: 100%;
    height: 100px;
  }
</style>
