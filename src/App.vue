<template>
  <div id="container">
    <Header></Header>
    <Loader v-if="!isLoading"></Loader>
    <ul>
      <li
        v-for="(todo, index) in todos"
        :key="todo.id"
        @dblclick="checkTask(index)"
      >
        <span :class="{ checked: todos[index].completed }"></span>
        {{ todo.title }}
        <i @click="deleteTask(index)" class="gg-close"></i>
      </li>
      <div class="clear"></div>
    </ul>
    <label class="titleForm" for="" v-if="isCanceled">
      <h1>Title</h1>
      <div>
        <input type="text" v-model="title" /><br />
        <button id="cancel-btn" @click="cancelTask">Cancel</button>
        <button id="add-btn" @click="addTask">Add</button>
      </div>
    </label>
    <div v-if="isLoading" class="plus-btn">
      <button @click="cancelTask" v-if="!isCanceled">+</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Header from "./components/Header.vue";
import Loader from "./components/Loader.vue";

export default {
  name: "App",

  data: function () {
    return {
      todos: [],
      title: "",
      isLoading: false,
      isCanceled: false,
    };
  },

  components: {
    Header,
    Loader,
  },

  created() {
    this.timeOut(
      3000,
      axios.get(
        "https://jsonplaceholder.typicode.com/todos?_start=0&_limit=5&_delay=3000"
      )
    )
      .then((response) => {
        this.isLoading = true;
        return response.data;
      })
      .then((todos) => {
        this.todos = todos;
      });
  },

  methods: {
    // delay 3s
    timeOut: function (ms, promise) {
      return new Promise(function (resolve) {
        setTimeout(function () {
          resolve(promise);
        }, ms);
      });
    },

    // add task
    addTask: function () {
      if (this.title.length === 0) {
        alert("Title is empty");
      } else {
        let task = {
          id: this.todos.length + 1,
          title: this.title,
          completed: false,
        };
        this.todos.push(task);
        this.title = "";
      }
    },

    // cancel task
    cancelTask: function () {
      if (this.isCanceled === false) {
        this.isCanceled = true;
      } else {
        this.isCanceled = false;
      }
      this.title = "";
    },

    // delete task
    deleteTask: function (index) {
      if (confirm("Are you sure you want to delete this!")) {
        this.todos.splice(index, 1);
      }
    },

    // check task
    checkTask: function (index) {
      if (this.todos[index].completed) {
        this.todos[index].completed = false;
      } else {
        this.todos[index].completed = true;
      }
    },
  },
};
</script>

<style>
@import "/src/assets/global.css";
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  justify-content: center;
}

body {
  background-color: #7084d5;
}

#container {
  position: relative;
  width: 50%;
  margin: 0 auto;
  border: 1px solid #fff;
  border-radius: 10px;
  margin-bottom: 50px;
}

#container ul {
  padding: 0;
  text-align: center;
}

li {
  position: relative;
  display: inline-block;
  width: 95%;
  list-style: none;
  padding: 20px 0;
  border-radius: 10px;
  margin: 8px auto;
  background-color: #fff;
}

#container ul li:hover {
  border: 2px solid #3cb371;
  background-color: #dad6d6;
}

.gg-close {
  display: none;
}

li:hover > .gg-close {
  display: inline-block;
  float: right;
  right: 20px;
  bottom: 2px;
  cursor: pointer;
}
.gg-close::after,
.gg-close::before {
  content: "";
  display: block;
  box-sizing: border-box;
  position: absolute;
  width: 25px;
  height: 5px;
  background: currentColor;
  transform: rotate(45deg);
  border-radius: 5px;
  top: 8px;
  left: 1px;
}
.gg-close::after {
  transform: rotate(-45deg);
}

.plus-btn {
  margin-top: 3%;
  margin-bottom: 3%;
}

.plus-btn button {
  background-color: #5c70be;
  font-size: 45px;
  border: 1px solid #5c70be;
  border-radius: 10px;
  color: #fff;
  padding: 5px 70px;
  line-height: 30px;
}

.titleForm h1 {
  margin: 20px 0;
}

.titleForm input {
  margin-bottom: 15px;
  padding: 15px;
  width: 50%;
  border-radius: 10px;
  border: none;
}

.titleForm #cancel-btn {
  background-color: #ccc;
  border: none;
  padding: 15px;
  font-size: 17px;
  border-radius: 10px;
  margin-right: 156px;
}

.titleForm #add-btn {
  background-color: #07b99d;
  border: none;
  padding: 15px;
  font-size: 17px;
  border-radius: 10px;
}

li .checked {
  height: 100%;
  width: 15px;
  left: 0;
  top: 0;
  position: absolute;
  background-color: #07b99d;
  border-radius: 8px 0 0 8px;
}
</style>
