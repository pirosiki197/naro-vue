<script setup lang="ts">
import { ref } from 'vue';

type Todo = {
    id: number
    name: string
    priority: number
    done: boolean
}

let id = 0
const todoList = ref<Todo[]>([])
const newTodoName = ref('')
const newTodoPriority = ref(1)

const addTodo = () => {
    if (newTodoName.value === '') return
    todoList.value.push({id: id++, name: newTodoName.value, priority: newTodoPriority.value, done: false})
    todoList.value.sort((a, b) => b.priority - a.priority)
    newTodoName.value = ''
    newTodoPriority.value = 1
}
</script>

<template>
    <div>
        <h1>ToDoリスト</h1>
        <h2>未完タスク</h2>
        <ul>
            <li v-for="todo in todoList.filter((t) => !t.done)" :key="todo.id" :class="{ 'urgent': todo.priority === 5 }">
                <div>
                    <button @click="todo.done=true">済</button>
                    {{ todo.name }}
                </div>
                <div>優先度: {{ todo.priority }}</div>
            </li>
        </ul>
        <h2>完了済みタスク</h2>
        <ul>
            <li v-for="todo in todoList.filter((t) => t.done)" :key="todo.id">
                {{ todo.name }}
            </li>
        </ul>
        <div>
            <label for="new-name">やること</label>
            <input type="text" v-model="newTodoName">
            <label for="new-priority">優先度</label>
            <input type="number" min="1" max="5" v-model="newTodoPriority">
            <button @click="addTodo">追加</button>
        </div>
    </div>
</template>

<style>
.urgent {
    color: red;
}
</style>