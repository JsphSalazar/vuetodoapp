<template>
  <div>
    <div class="section no-pad-bot" id="index-banner">
      <div class="container">
        <h3 class="header center black-text">My To-Do App</h3>
        <form @submit.prevent="addTask">
          <div class="row center">
            <h6 class="header col s12 light">This is supposed to be a good title</h6>
          </div>
          <input v-model="newTaskName" type="text" placeholder="Add a new task  ">
        <div class="row center">
          <button class="btn-small waves-effect waves-light red" type="submit">Submit <i class="material-icons right">send</i></button>
        </div>
        </form>
      </div>
    </div>
    <div class="container">
      <div class="section">
        <div class="row">
          <div class="col s12 m6">
            <div class="card-panel red">
              <span class="white-text">
                <center><i class="material-icons">alarm</i></center>
                <h5 class="center">To-Do! </h5>
                <tasks-list
                  :task-list="tasksPending"
                  @completar="toggleTasks"/>
              </span>
            </div>
          </div>
          <div class="col s12 m6">
            <div class="icon-block">
              <div class="card-panel green">
                <span class="white-text">
                  <center><i class="material-icons">done_outline</i></center>
                  <h5 class="center">Done</h5>
                  <tasks-list
                    :task-list="tasksComplete"
                    @completar="toggleTasks"/>
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TasksList from './components/TasksList'
import Firebase from 'firebase'

let config = {
  apiKey: "AIzaSyCH9EmhMIZWN29mNoMAaih6XhgLRDLiClg",
  authDomain: "vuejs-to-do-app.firebaseapp.com",
  databaseURL: "https://vuejs-to-do-app.firebaseio.com",
  projectId: "vuejs-to-do-app",
  storageBucket: "vuejs-to-do-app.appspot.com",
  messagingSenderId: "236009848028"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let tasksRef = db.ref('tasks');


export default {
  components: {
    TasksList
  },
  firebase: {
    tasks: tasksRef
  },
  data () {
    return {
      newTaskName: '',
      tasks: [
        {
          name: 'Tarea 1',
          done: false
        },
        {
          name: 'Tarea 2',
          done: false
        },
        {
          name: 'Tarea 2',
          done: false
        }
      ]
    }
  },
  methods: {
    toggleTasks (task) {
      task.done = !task.done
    },
    addTask () {
      if (!this.newTaskName) return
      tasksRef.push({ name: this.newTaskName, done: false })
      this.newTaskName = ''
    }
  },
  computed: {
    tasksPending () {
      return this.tasks.filter(task => !task.done)
    },
    tasksComplete () {
      return this.tasks.filter(task => task.done)
    }
  }
}
</script>
