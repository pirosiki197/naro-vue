<script setup lang="ts">
import { ref, watch } from 'vue';

type Todo = {
    id: number
    name: string
    priority: number
    done: boolean
}

const id = ref<number>(parseInt(localStorage.getItem('id') || "0", 10))
const todoList = ref<Todo[]>(JSON.parse(localStorage.getItem('todoList') || "[]"))
const newTodoName = ref('')
const newTodoPriority = ref(1)

watch(id, (newId) => {
    localStorage.setItem("id", String(newId))
})

watch(todoList, (newTodoList) => {
    localStorage.setItem("todoList", JSON.stringify(newTodoList))
})

const addTodo = () => {
    if (newTodoName.value === '') return
    id.value += 1
    const newTodo: Todo = {
        id: id.value,
        name: newTodoName.value,
        priority: newTodoPriority.value,
        done: false
    }
    const newTodoList: Todo[] = [...todoList.value, newTodo]
    newTodoList.sort((a, b) => b.priority - a.priority)
    todoList.value = newTodoList
    newTodoName.value = ''
    newTodoPriority.value = 1
}

const completeTask = (id: number) => {
    const newTodoList = todoList.value.map((todo) => {
        if (todo.id === id) {
            return {...todo, done: true}
        }
        return todo
    })
    todoList.value = newTodoList
}
</script>

<template>
    <div>
        <h1>ToDoリスト</h1>
        <h2>未完タスク</h2>
        <ul>
            <li v-for="todo in todoList.filter((t) => !t.done)" :key="todo.id" :class="{ 'urgent': todo.priority === 5 }">
                <div>
                    <button @click="completeTask(todo.id)">済</button>
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