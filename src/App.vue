<script setup>
import TodoListInput from "./components/TodoListInput.vue";
import TodoListItem from "./components/TodoListItem.vue";
import { onMounted, ref } from "vue";

const todosArray = ref([]);
const getItem = ref({
  id: "",
  item: "",
});
onMounted(() => {
  let retrievedData = localStorage.getItem("todosArray");
  todosArray.value = JSON.parse(retrievedData);
  //console.log(todosArray.value);
});
const localStore = () => {
  localStorage.setItem("todosArray", JSON.stringify(todosArray.value));
};
const save = (i) => {
  const todos = {
    id: "",
    item: "",
    completed: false,
  };
  todos.item = i;
  const getRandomNumber = (min, max) => {
    return Math.random() * (max - min) + min;
  };

  todos.id = getRandomNumber(0, 10000);
  if (todosArray.value.length === 0) todosArray.value.push(todos);
  else {
    let checkId = todosArray.value.some((i) => {
      return i.id === todos.id;
    });
    if (checkId) {
      todos.id = getRandomNumber(0, 10000);
    }
    todosArray.value.push(todos);
    if (todosArray.value.length !== 0) {
      localStore();
    }
  }
};

const deleteTodoItem = (i) => {
  const todoItemSearch = todosArray.value.find((todo) => {
    return i === todo.id;
  });
  //console.log(todoItemSearch);
  let index = todosArray.value.indexOf(todoItemSearch);
  //console.log(index);
  todosArray.value.splice(index, 1);
  localStore();
};

const completeTodoItem = (i) => {
  const completedItem = todosArray.value.find((todo) => {
    return i === todo.id;
  });
  let index = todosArray.value.indexOf(completedItem);
  //console.log(completedItem);
  todosArray.value[index].completed = true;
  //console.log(todosArray.value)
  localStore();
};

const getTodoItem = (i) => {
  const getItems = todosArray.value.filter((todo) => {
    return i === todo.id;
  });
  //console.log(getItems[0].item);
  getItem.value.item = getItems[0].item;
  getItem.value.id = i;
};

const updateTodoItem = (i, j) => {
  //console.log(i,j);
  const updatedItem = todosArray.value.find((todo) => {
    return j === todo.id;
  });
  let index = todosArray.value.indexOf(updatedItem);
  //console.log(index);
  todosArray.value[index].item = i;
  //console.log(todosArray.value[index].item, i);
  localStore();
};
const completedTodoItem = () => {
  let completedItem = [];
  completedItem = todosArray.value.filter((todo) => {
    return todo.completed === true;
  });
  todosArray.value = completedItem;
  //console.log(todosArray.value);
};
const allTodoItem=()=>{
  let retrievedData = localStorage.getItem("todosArray");
  todosArray.value = JSON.parse(retrievedData);
}
</script>

<template>
  <div id="container">
    <TodoListInput
      @save="save"
      @update="updateTodoItem"
      :updateTodo="getItem"
    />
    <!-- <ul v-if="todosArray.length != 0">
    <li v-for="i in todosArray">{{ i.item }}</li>
  </ul> -->
    <section :style="`display:${todosArray.length ? 'block' : 'none'}`">
      <ul v-if="todosArray.length != 0">
        <div id="completedTodos">
          <button @click="completedTodoItem">Completed</button>
          <button @click="allTodoItem" style="width: 60px;">All</button>
        </div>
        <TodoListItem
          v-for="i in todosArray"
          :key="i.id"
          :todo.sync="i"
          @close="deleteTodoItem"
          @complete="completeTodoItem"
          @get="getTodoItem"
        />
      </ul>
    </section>
  </div>
</template>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body {
  background-color: rgb(5, 5, 70);
}
#completedTodos{
  margin-left: auto;
  margin-right: auto;
  width: 200px;
  margin-top: 10px;
  margin-bottom: 20px;;
}
#completedTodos button:hover{
  background-color: red;
}
ul {
  padding-right: 20px;
  padding-left: 20px;
  max-height: 400px;
  flex: 1;
  overflow-y: auto;
}
section {
  border-radius: 5%;
  margin-top: 40px;
  width: max-content;
  margin-left: auto;
  margin-right: auto;
  padding: 20px;
  background-color: rgb(12, 111, 197);
}
@media screen and (max-width: 450px) {
  section {
    width: 100%;
  }
  ul {
    width: 100%;
    padding: 0px;
  }
}
</style>
