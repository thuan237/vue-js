<!-- eslint-disable vue/require-v-for-key -->
<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up,
        <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>
    <section class="create-todo">
      <h3>CREATE A TODO</h3>
      <form @submit.prevent="addToDo">
        <h4>What's on your to do list?</h4>
        <input
          type="text"
          placeholder="e.g. make a video"
          v-model="input_content"
        />
        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              value="business"
              v-model="input_category"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input
              type="radio"
              name="category"
              value="personal"
              v-model="input_category"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
        <input type="submit" value="Add todo" />
      </form>
    </section>

    <section>
      <h3>TO DO LIST</h3>
      <div class="list" id="todo-list">
        <div
          v-for="todo in todos"
          :class="`todo-item ${todo.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span
              :class="`bubble ${
                todo.category == 'business' ? 'business' : 'personal'
              }`"
            ></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";

const name = ref("");
const input_category = ref(null);
const input_content = ref("");

const todos = ref([]);

const addToDo = () => {
  if (input_category.value === null || input_content.value.trim() === "") {
    return;
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime(),
  });
  console.log("add successfully");
};

// const todos_asc = computed(() =>
//   todos.value.sort((a, b) => {
//     return a.createdAt - b.createdAt;
//   })
// );

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

watch(
  todos,
  (newVal) => {
    console.log('new value: ', JSON.stringify(newVal));
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  {
    deep: true,
  }
);

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>
