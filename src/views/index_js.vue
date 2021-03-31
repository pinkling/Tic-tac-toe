<template>
  <div style="background: #3a534d;width: 100vw;height: 100vh;">
    <div class="title"
         @click="handleInit">
      小游戏
    </div>

    <div class="content">
      <div @click="handlePlayChess(i)"
           class="cell"
           :class="['cell' + checkerboard[i - 1], checkerboard[i - 1] === '' ? 'pre' + role : '']"
           v-for="(i, index) in 9"
           :key="index">
      </div>
      <div class="message" v-show="gameStatus === 'end'">
        {{role}}赢了!

        <div class="btn"
             @click="handleInit">
          重新开始
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'index',
  data() {
    return {
      // 角色 当前下什么棋
      role: 'O',
      // 棋盘 存放放置棋子
      checkerboard: ['', '', '', '', '', '', '', '', ''],
      gameStatus: 'start'
    }
  },
  methods: {
    handleInit() {
      this.role = 'O',
      this.checkerboard = ['', '', '', '', '', '', '', '', ''],
      this.gameStatus = 'start'
    },
    handlePlayChess(index) {
      // 更新棋盘样式
      // 调整棋盘数据
      if (this.checkerboard[index - 1] === '') {
        this.$set(this.checkerboard, index - 1, this.role)
        this.checkerboard[index - 1] = this.role
      } else {
        alert('请勿重复填写数据')
        return
      }
      // 判断是否胜利
      this.handleJudgment()
    },
    changeRole() {
      this.role = this.role === 'O' ? 'X' : 'O'
    },
    handleJudgment() {
      const isWin = this.judgmentByRole(this.role)
      if (isWin) {
        this.gameStatus = 'end'
      } else {
        this.changeRole()
      }
    },
    judgmentByRole(role) {
      const flag = [
          [0, 1, 2],
          [3, 4, 5],
          [6, 7, 8],
          [0, 3, 6],
          [1, 4, 7],
          [2, 5, 8],
          [0, 4, 8],
          [2, 4, 6],
      ]
      for (const i of flag) {
        let status = true
        for (const j of i) {
          if (this.checkerboard[j] !== role) {
            status = false
            break
          }
        }
        if (status === true) {
          return true
        }
      }
      return false
    }
  }
}
</script>

<style scoped lang="scss">
.title {
  font-family: "Adobe 黑体 Std R";
  font-size: 56px;
  color: #fff;
  letter-spacing: 5px;
  padding: 20px;
}
.content {
  margin: 0 auto;
  width: 900px;
  height: 900px;
  display: flex;
  flex-wrap: wrap;
  position: relative;

  .cell {
    width: 300px;
    height: 300px;
    box-sizing: border-box;
    border: 5px #19c2ef solid;
    background: rgba(255, 255, 255, 0.2);
    transition: all .3s ease;
    cursor: pointer;
    position: relative;

    &:hover {
      background: rgba(255, 255, 255, 0.1);
    }

    &:nth-child(3n + 1) {
      border-left: none;
    }
    &:nth-child(3n + 3) {
      border-right: none;
    }
    &:nth-child(-n + 3) {
      border-top: none;
    }
    &:nth-child(-n + 9):nth-child( n + 7) {
      border-bottom: none;
    }
  }

  .cellO {
    &:before {
      content: '';
      width: 150px;
      height: 150px;
      box-sizing: border-box;
      border: 5px solid #ffd100;
      border-radius: 50%;
      position: absolute;
      left: calc(50% - 75px);
      top: calc(50% - 75px);
    }
  }
  .cellX {
    &:before {
      content: '';
      width: 150px;
      height: 150px;
      box-sizing: border-box;
      border: 5px solid #00e03a;
      position: absolute;
      left: calc(50% - 75px);
      top: calc(50% - 75px);
    }
  }
  .preO {
    &:hover:before {
      content: '';
      width: 150px;
      height: 150px;
      box-sizing: border-box;
      border: 5px solid #ffd10066;
      border-radius: 50%;
      position: absolute;
      left: calc(50% - 75px);
      top: calc(50% - 75px);
    }
  }
  .preX {
    &:hover:before {
      content: '';
      width: 150px;
      height: 150px;
      box-sizing: border-box;
      border: 5px solid #00e03a66;
      position: absolute;
      left: calc(50% - 75px);
      top: calc(50% - 75px);
    }
  }
  .message {
    width: 900px;
    height: 900px;
    line-height: 900px;
    background: rgba(29, 43, 40, 0.6);
    font-size: 100px;
    color: #FFF;
    text-align: center;
    position: absolute;
    left: 0;
    top: 0;
    .btn {
      width: 200px;
      height: 60px;
      line-height: 60px;
      font-size: 30px;
      background: #19c2ef;
      color: #FFF;
      border: 3px solid #00e03a;
      position: absolute;
      left: calc(50% - 100px);
      top: 600px;
      cursor: pointer;
    }
  }
}

</style>
