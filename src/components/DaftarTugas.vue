<template>
    <div class="container">
      <h1>DAFTAR TUGAS SAYA</h1>
      <form @submit.prevent="addTask" class="task-form">
        <input type="text" v-model="newTask" placeholder="Tambahkan tugas" />
        <button type="submit">Tambah</button>
      </form>
      <ul>
        <li v-for="(task, index) in filteredTasks" :key="index">
          <input type="checkbox" :checked="task.done" @change="toggleDone(task)" />
          <span :class="{ done: task.done }">{{ task.text }}</span>
          <button @click="deleteTask(index)" class="delete-button">Hapus</button>
        </li>
      </ul>
      <div class="filter">
        <label class="switch">
          <input type="checkbox" v-model="showCompleted" class="custom-checkbox" />
          <span class="slider"></span>
        </label>
        <span class="filter-label">Tampilkan yang sudah selesai</span>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "DaftarTugas",
    data() {
      return {
        newTask: "",
        tasks: [],
        showCompleted: true, // Ubah nilai default menjadi true
      };
    },
    created() {
      this.loadTasksFromStorage();
    },
    computed: {
      filteredTasks() {
        if (this.showCompleted) {
          return this.tasks;
        } else {
          return this.tasks.filter(task => !task.done);
        }
      },
    },
    methods: {
      addTask() {
        if (this.newTask.trim()) {
          this.tasks.push({ text: this.newTask, done: false });
          this.saveTasksToStorage();
          this.newTask = "";
        }
      },
      deleteTask(index) {
        this.tasks.splice(index, 1);
        this.saveTasksToStorage();
      },
      toggleDone(task) {
        task.done = !task.done;
        this.saveTasksToStorage();
      },
      saveTasksToStorage() {
        localStorage.setItem("tasks", JSON.stringify(this.tasks));
      },
      loadTasksFromStorage() {
        const storedTasks = localStorage.getItem("tasks");
        if (storedTasks) {
          this.tasks = JSON.parse(storedTasks);
        }
      },
    },
  };
  </script>

<style>
.container {
  text-align: center;
}

.done {
  text-decoration: line-through;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  margin-bottom: 20px;
  margin-top: 10px;
}

input[type="text"] {
  padding: 5px;
  border: none;
  border-radius: 5px;
  margin-right: 5px;
}

button {
  padding: 5px 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #3e8e41;
}

.done {
  text-decoration: line-through;
}

.custom-checkbox {
  display: inline-block;
  position: relative;
  width: 16px;
  height: 16px;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 3px;
  cursor: pointer;
}

.custom-checkbox:checked::before {
  content: "";
  position: absolute;
  top: 2px;
  left: 2px;
  width: 10px;
  height: 10px;
  background-color: #333;
  border-radius: 2px;
}

.switch {
  position: relative;
  display: inline-block;
  width: 40px;
  height: 20px;
}

.switch .custom-checkbox {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  border-radius: 20px;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 16px;
  width: 16px;
  left: 2px;
  bottom: 2px;
  background-color: #fff;
  border-radius: 50%;
  transition: 0.4s;
}

.custom-checkbox:checked + .slider {
  background-color: #4CAF50;
}

.custom-checkbox:checked + .slider:before {
  transform: translateX(20px);
}

.filter-label {
  margin-left: 10px;
}

.delete-button {
  margin-left: 10px;
}

.task-form {
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 20px;
  border-radius: 5px;
}
</style>

