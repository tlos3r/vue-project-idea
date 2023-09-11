<template>
    <h1 class="mt-20 text-5xl text-center">Very Simple Todo App</h1>
    <div class="flex flex-col items-center justify-center mt-10">
        <form @submit.prevent="addTodo" class="flex">
            <input type="text" class="w-full border-4 focus:outline-none" v-model="note" />
            <button class="px-3 py-1 text-white w-36 bg-zinc-400">Submit</button>
        </form>
        <p ref="notiref" class="font-bold text-center text-red-600"></p>
        <ul class="mt-3">
            <li v-for="(todo, index) in todos" :key="index" class="flex items-center p-2">
                <p class="px-3" :class="{ checked: todo.finishes }">{{ todo.text }}</p>
                <input type="checkbox" v-model="todo.finishes" class="w-5 h-5 mx-3" />
                <button class="px-2 py-1 text-white bg-red-500" @click="removeTodo(index)">del</button>
                <button class="px-2 py-1 mx-1 text-white bg-blue-500" @click="updateTodo(index)">Update</button>
            </li>
        </ul>
    </div>
</template>

<script setup lang="ts">
import { reactive, ref, type Ref } from "vue";
const note = ref("");
const todos = reactive([
    {
        text: "simple ex",
        finishes: false,
    },
]);
const notiref: Ref<any> = ref(null);
const addTodo = () => {
    if (note.value.length <= 0) {
        notiref.value.textContent = "You dont have any todo";
    } else {
        notiref.value.textContent = "";
        todos.push({ text: note.value, finishes: false });
        note.value = "";
    }
};

const removeTodo = (id: number) => {
    todos.splice(id, 1);
};

const updateTodo = (index: number) => {
    todos[index].text = note.value;
};
</script>

<style scoped>
.checked {
    @apply line-through;
}
</style>
