<template>
  <div class="todo-list">
    <div>
      <label>新增待办</label>
      <input v-model="state.todo" @keyup.enter="handleAddTodo">
    </div>
    <div>
      <h3>待办列表({{todos.length}})</h3>
      <ul>
        <li v-for="item in todos" :key="item.id" @click="handleChangeStatus(item, true)">
          <input type="checkbox">
          <label>{{item.text}}</label>
        </li>
      </ul>
    </div>
    <div><h3>已办列表({{dones.length}})</h3></div>
    <ul>
      <li v-for="item in dones" :key="item.id" @click="handleChangeStatus(item, false)">
          <input type="checkbox" checked>
          <label>{{item.text}}</label>
        </li>
    </ul>
    <a-button type="primary" loading>
      Primary
    </a-button>
    <a-button type="primary" :loading="state.loading">
      mouseenter me!
    </a-button>
  </div>
</template>

<script lang="ts">
import { reactive, computed } from 'vue'

interface TodoVM {
  id: number;
  done: boolean;
  text: string;
}

export default{ 
  // setup相当于vue2.0的 beforeCreate和 created，是vue3新增的一个属性，所有的操作都在此属性中完成
  setup(props: any, context: any) {
    const todoList: TodoVM[] = [
      {
        id: 1,
        done: false,
        text: '吃饭'
      },
      {
        id: 2,
        done: false,
        text: '睡觉'
      }
    ]
    const state = reactive({
      todoList: todoList,
      todo: '',
      loading: false
    })

    // 使用计算属性生成待办列表
    const todos = computed(() => {
      return state.todoList.filter(item => !item.done)
    })


    // 使用计算属性生成已办列表
    const dones = computed(() => {
      return state.todoList.filter(item => item.done)
    })

    // 修改待办状态
    const handleChangeStatus = (item: TodoVM, status: boolean) => {
      item.done = status
    }

    const handleAddTodo = () => {
      if(!state.todo) {
        alert('请输入待办')
        return
      }

      state.todoList.push({
        text: state.todo,
        id: Date.now(),
        done: false
      })
      state.todo = ''
    }

    return {
      state,
      todos,
      dones,
      handleChangeStatus,
      handleAddTodo
    }
  }
}
</script>

<style scoped>
.todo-list{
  text-align: center;
}

.todo-list ul li {
  list-style: none;
}
</style>