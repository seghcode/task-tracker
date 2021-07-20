<template>
  <div v-show="showAddTask">
    <AddTask @add-task="addTask" />
  </div>
  <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
</template>

<script>
import Tasks from "../components/Tasks"
import AddTask from "../components/AddTask"

export default {
  name: "Home",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      Tasks: [],
    };
  },
  methods: {
    async addTask(task) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });

      const data = await res.json();

      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id) {
      // console.log('task', id);
      if (confirm("Are you sure")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });

        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error Deleting Task");
      }
    },
    async toggleReminder(id) {
      // console.log(id);
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask),
      });

      const data = await res.json();

      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
    async fetchTasks() {
      const res = await fetch("api/tasks");

      const data = await res.json();

      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);

      const data = await res.json();

      return data;
    },
  },
  async created() {
    /* getting our data from the json db */
    this.tasks = await this.fetchTasks();
    /* this.tasks = [
      {
        id: 1,
        text: "Doctors Appointment",
        day: "July 17 2021",
        reminder: true,
      },
      {
        id: 2,
        text: "School Visit",
        day: "July 27 2021",
        reminder: true,
      },
      {
        id: 3,
        text: "Travel to Lamingo",
        day: "July 20 2021",
        reminder: false,
      }
    ] */
  },
};
</script>
