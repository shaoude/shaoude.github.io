<template>
  <div class="Todo">
    <div class="Todo-header">
      <h1>{{ title }}</h1>
      <input class="new-todo" @keyup.enter="addTodo" placeholder="计划将要去做的事情......" autofocus>
    </div>

    <div class="Todo-main" v-if="todoList.length">
      <ul class="todo-list">
        <li v-for="(v,k) in todoList" :class="{completed: v.isCompleted}" :key='v.id'>
          <input class="toggle" type="checkbox" v-model="v.isCompleted" @click="todoDone(k, v.id)">
          <label @click="todoDone(k, v.id)">{{ v.content }}</label>
          <button @click="removeTodo(k, v.id)" class="destroy"></button>
        </li>
      </ul>
    </div>

    <div class="Todo-footer">
      <span class="todo-count"><span>{{ notCompleted }}</span> 项剩余未完成</span>
      <ul class="filters">
        <li>
          <span class="selected">全部</span>
        </li>
        <li>
          <span>未完成</span>
        </li>
        <li>
          <span>已完成</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todo',
  data () {
    return {
      title: 'TodoList',
      todoList: [
        {id: 1, content: '一起给对方写信，然后读给对方听', isCompleted: true},
        {id: 2, content: '一起看书，分享自己喜欢的书籍', isCompleted: false},
        {id: 3, content: '一起入住一次五星级酒店，看夜景', isCompleted: true}
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
        id = this.todoList[len - 1]['id'] + 1
      }
      let isCompleted = false
      this.todoList.push({id, content, isCompleted})
      ev.target.value = ''
    },
    todoDone (ind, id) {
      this.todoList[ind].isCompleted = !this.todoList[ind].isCompleted
    },
    removeTodo (ind, id) {
      this.todoList.splice(ind, 1)
    }
  }
}
</script>

<style lang='less' scoped>
.Todo {
  background: #fff;
  margin: 130px 0 40px 0;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2),
              0 25px 50px 0 rgba(0, 0, 0, 0.1);
}

  .new-todo {
    position: relative;
    margin: 0;
    width: 100%;
    font-size: 24px;
    font-family: inherit;
    font-weight: inherit;
    line-height: 1.4em;
    color: inherit;
    padding: 6px;
    border: 1px solid #999;
    box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
    box-sizing: border-box;
    padding: 16px 16px 16px 60px;
    border: none;
    background: rgba(0, 0, 0, 0.003);
    box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
  }

  h1 {
    position: absolute;
    top: -140px;
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

  .Todo-main {
    position: relative;
    z-index: 2;
    border-top: 1px solid #e6e6e6;
    .todo-list {
      margin: 0;
      padding: 0;
      list-style: none;
    }

    .todo-list li {
      position: relative;
      font-size: 24px;
      border-bottom: 1px solid #ededed;
    }

    .todo-list li:last-child {
      border-bottom: none;
    }

    .todo-list li .toggle {
      text-align: center;
      width: 40px;
      height: auto;
      position: absolute;
      top: 0;
      bottom: 0;
      margin: auto 0;
      border: none;
      appearance: none;
    }

    .todo-list li .toggle {
      opacity: 0;
    }

    .todo-list li .toggle + label {
      background-image: url('../../assets/notCompleted.png');
      background-size:40px 40px;
      background-repeat: no-repeat;
      background-position: center left;
    }

    .todo-list li .toggle:checked + label {
      background-image: url('../../assets/completed.png');
    }

    .todo-list li label {
      word-break: break-all;
      padding: 15px 15px 15px 60px;
      display: block;
      line-height: 1.2;
      transition: color 0.4s;
      font-weight: 400;
      color: #4d4d4d;
    }

    .todo-list li.completed label {
      color: #cdcdcd;
      text-decoration: line-through;
    }

    .todo-list li .destroy {
      display: none;
      position: absolute;
      top: 0;
      right: 10px;
      bottom: 0;
      width: 40px;
      height: 40px;
      margin: auto 0;
      font-size: 30px;
      color: #cc9a9a;
      margin-bottom: 11px;
      transition: color 0.2s ease-out;
    }

    .todo-list li .destroy:hover {
      color: #af5b5e;
    }

    .todo-list li .destroy:after {
      content: '×';
    }

    .todo-list li:hover .destroy {
      display: block;
    }
  }

  .Todo-footer {
    padding: 10px 15px;
    height: 20px;
    text-align: center;
    font-size: 15px;
    border-top: 1px solid #e6e6e6;
  }
  .Todo-footer:before {
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
  .todo-count {
    float: left;
    text-align: left;
  }

  .filters {
    margin: 0;
    padding: 0;
    list-style: none;
    position: absolute;
    right: 0;
    left: 0;
  }

  .filters li {
    display: inline;
  }

  .filters li span {
    color: inherit;
    margin: 3px;
    padding: 3px 7px;
    text-decoration: none;
    border: 1px solid transparent;
    border-radius: 3px;
  }

  .filters li span:hover {
    border-color: rgba(175, 47, 47, 0.1);
  }

  .filters li span.selected {
    border-color: rgba(175, 47, 47, 0.2);
  }
</style>
