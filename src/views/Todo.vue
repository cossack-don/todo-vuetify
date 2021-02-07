<template>
  <div class="home">
    <v-text-field
      v-model.trim="newTaskTitle"
      @click:append="addTask"
      @keyup.enter="addTask"
      class="pa-3"
      outlined
      label="Append"
      append-icon="mdi-plus"
      hide-details
      clearable
    ></v-text-field>

    <div class="my-2 mb-4 d-flex justify-center">
      <v-btn color="blue-grey" @click="DeleteAllTasks"> Delet all tasks </v-btn>
    </div>

    <v-list flat class="pt-0">
      <div v-for="task in tasks" :key="task.id">
        <v-list-item
          @click="doneTask(task.id)"
          :class="{ 'blue lighten-5': task.completed }"
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox
                :input-value="task.completed"
                color="primary"
              ></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{ 'text-decoration-line-through': task.completed }"
              >
                {{ task.title }}</v-list-item-title
              >
            </v-list-item-content>

            <v-list-item-action>
              <v-btn icon @click.stop="deteleTask(task.id)">
                <v-icon color="primary lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>
    </v-list>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      newTaskTitle: "",
      tasks: [
        // {
        //   id: 1,
        //   title: "wake up",
        //   done: false,
        // },
        // {
        //   id: 2,
        //   title: "get 2",
        //   done: false,
        // },
        // {
        //   id: 3,
        //   title: "task 3",
        //   done: false,
        // },
      ],
    };
  },
  mounted() {
    // test
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=10")
      .then((res) => {
        // console.log(res);
        return res.json();
      })
      .then((data) => {
        this.tasks = data;
      });
  },
  methods: {
    addTask() {
      // console.log("addTask");
      if (this.newTaskTitle === "") {
        return;
      }

      let newTask = {
        id: Date.now(),
        title: this.newTaskTitle,
        completed: false,
      };

      this.tasks.push(newTask);
      this.newTaskTitle = "";
    },

    doneTask(id) {
      // console.log(id);
      let task = this.tasks.filter((task) => task.id === id)[0];
      task.completed = !task.completed;
    },

    deteleTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },

    DeleteAllTasks() {
      this.tasks = [];
    },
  },
};
</script>
