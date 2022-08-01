<template>
  <h1>{{ title }}</h1>

  <img :src="logoURL" :alt="logoCaption" width="100" height="100">

  <h2>Add a new task</h2>

  <span>You have {{allTasks}} {{allTasks > 1 ? 'tasks' : 'task'}} at the moment.</span>

  <div>
    <input type="text" v-model="newTask" placeholder="Add a new task" @keyup.enter="addTask">
  </div>

  <button @click="addTask" :disabled="newTask.length < 1">
    Add task
  </button>

  <div v-if="newTask.length > 0">
    <h3>New task preview</h3>
    <p>{{newTask}}</p>
  </div>

  <ul>
    <li v-for="(task, index) in latest" :key="task.id"
      @click="finishTask(task)"
      :class="{strikeout: task.finished}"
    >
      {{index + 1}}. {{task.name}}

      <div v-if="task.finished">
        <button @click="removeTask(task.id)">Delete task</button>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      title: 'My toDo App',
      newTask: '',
      tasks: [
        { id: 1, name: 'Learn VueJS', finished: true },
        { id: 2, name: 'Build a Vue application', finished: false },
        { id: 3, name: 'Write an article about Vue JS', finished: false }
      ],
      logoURL: 'https://images.unsplash.com/photo-1507925921958-8a62f3d1a50d?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1955&q=80',
      logoCaption: 'A photo by Kelly Sikkema on Unsplash showing post-it notes'
    }
  },
  methods: {
    addTask() {
      if(this.newTask.length < 1) return 

      this.tasks.push({
        id: this.tasks.length + 1,
        name: this.newTask,
        finished: false
      });

      this.newTask = '';
    },
    removeTask(taskID) {
      this.tasks = this.tasks.filter(
        task => task.id !== taskID
      );
    },
    finishTask(task) {
      task.finished = !task.finished
    }
  },
  computed: {
    allTasks() {
      return this.tasks.length
    },
    latest() {
      return [...this.tasks].reverse();
    }
  }
}
</script>

<style>
  .strikeout {
    text-decoration: line-through;
  }

  button:hover, li:hover {
    cursor: pointer;
  }
</style>
