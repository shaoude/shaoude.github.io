<template>
  <div class="Todo">
    <div class="Todo-header">
      <h1>{{ title }}</h1>
      <input class="Todo-add" @keyup.enter="addTodo" placeholder="计划将要去做的事情......" autofocus>
    </div>

    <div class="Todo-body" v-if="todoList.length">
      <ul class="Todo-list">
        <li v-for="(v,k) in todoList" :class="{completed: v.isCompleted}" :key='v.id'>
          <input class="Todo-toggle" type="checkbox" v-model="v.isCompleted" @click="toggleTodo(k)">
          <label @click="toggleTodo(k)">{{ v.content }}</label>
          <span @click="deleteTodo(k)" class="Todo-detete"></span>
        </li>
      </ul>
    </div>

    <div class="Todo-footer">
      <span class="Todo-count">
        <span class="Todo-notCompleted">{{ notCompleted }}</span> 项剩余未完成
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todo',
  data () {
    return {
      title: '待办事项',
      todoList: [
        {id: 3, content: '一起给对方写信，然后读给对方听', isCompleted: true},
        {id: 2, content: '一起看书，分享自己喜欢的书籍', isCompleted: false},
        {id: 1, content: '一起入住一次五星级酒店，看夜景', isCompleted: true}
      ]
    }
  },
  computed: {
    notCompleted () {
      let notCompletedList = this.todoList.filter((v) => !v.isCompleted)
      return notCompletedList.length
    }
  },
  methods: {
    addTodo (ev) {
      let content = ev.target.value
      let len = this.todoList.length
      let id = 0
      if (len) {
        id = this.todoList[0]['id'] + 1
      }
      let isCompleted = false
      this.todoList.unshift({id, content, isCompleted})
      ev.target.value = ''
    },
    toggleTodo (k) {
      this.todoList[k].isCompleted = !this.todoList[k].isCompleted
    },
    deleteTodo (k) {
      this.todoList.splice(k, 1)
    }
  }
}
</script>

<style lang='less' scoped>
.Todo {
  background: #fff;
  margin: 130px auto 40px auto;
  width: 50%;
  min-width: 440px;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2),
              0 25px 50px 0 rgba(0, 0, 0, 0.1);
  .Todo-header {
    h1 {
      position: absolute;
      top: -160px;
      width: 100%;
      font-size: 80px;
      font-weight: 200;
      text-align: center;
      color: #b83f45;
      text-rendering: optimizeLegibility;
    }
    input::-webkit-input-placeholder {
      font-style: italic;
      font-weight: 300;
      color: rgba(0, 0, 0, 0.4);
    }
    .Todo-add {
      position: relative;
      margin: 0;
      width: 100%;
      font-size: 24px;
      font-family: inherit;
      font-weight: inherit;
      line-height: 1.4em;
      color: inherit;
      box-sizing: border-box;
      padding: 16px 16px 16px 60px;
      border: none;
      background: rgba(0, 0, 0, 0.003);
      box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
    }
  }
  .Todo-body {
    position: relative;
    z-index: 2;
    border-top: 1px solid #e6e6e6;
    max-height: 330px;
    overflow-y: auto;
    .Todo-list {
      margin: 0;
      padding: 0;
      list-style: none;
      li {
        position: relative;
        font-size: 24px;
        border-bottom: 1px solid #ededed;
        &:last-child {
          border-bottom: none;
        }
        .Todo-toggle {
          text-align: center;
          width: 40px;
          height: auto;
          position: absolute;
          top: 0;
          bottom: 0;
          margin: auto 0;
          border: none;
          appearance: none;
          opacity: 0;
        }
        .Todo-toggle + label {
          background-image: url('../../assets/notCompleted.png');
          background-size:40px 40px;
          background-repeat: no-repeat;
          background-position: 10px 10px;
        }
        .Todo-toggle:checked + label {
          background-image: url('../../assets/completed.png');
        }
        label {
          word-break: break-all;
          padding: 15px 15px 15px 60px;
          display: block;
          line-height: 1.2;
          transition: color 0.4s;
          font-weight: 400;
          color: #4d4d4d;
          overflow: hidden;
          text-overflow:ellipsis;
          white-space: nowrap;
          cursor: pointer;
        }
        &.completed label {
          color: #cdcdcd;
          text-decoration: line-through;
        }
        .Todo-detete {
          display: none;
          position: absolute;
          top: 0;
          right: 10px;
          bottom: 0;
          width: 40px;
          height: 33px;
          margin: auto 0;
          font-size: 30px;
          color: #cc9a9a;
          margin-bottom: 11px;
          transition: color 0.2s ease-out;
          cursor: pointer;
          &:hover {
            color: #af5b5e;
          }
          &:after {
            content: '×';
          }
        }
        &:hover {
          .Todo-detete {
            display: block;
          }
        }
      }
    }
  }
  .Todo-footer {
    padding: 10px 15px;
    height: 30px;
    text-align: center;
    font-size: 15px;
    border-top: 1px solid #e6e6e6;
    &:before {
      content: '';
      position: absolute;
      right: 0;
      bottom: 0;
      left: 0;
      height: 50px;
      overflow: hidden;
      box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2),
                  0 8px 0 -3px #f6f6f6,
                  0 9px 1px -3px rgba(0, 0, 0, 0.2),
                  0 16px 0 -6px #f6f6f6,
                  0 17px 2px -6px rgba(0, 0, 0, 0.2);
    }
    .Todo-count {
      float: left;
      text-align: left;
      .Todo-notCompleted {
        font-size: 25px;
        color: red;
      }
    }
  }
}
@media (max-width: 430px) {
  .Todo {
    width: 100%;
    min-width: 0;
    .Todo-header h1 {
      font-size: 66px;
    }
    label {
      width:70%;
    }
  }
}
</style>
