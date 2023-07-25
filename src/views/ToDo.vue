<template>
  <div class="home">
    <v-text-field
        v-model="newTaskTitle"
        @keyup.enter="addTask"
        class="pa-3"
        outlined
        label="Add Task"
        append-icon="mdi-plus"
        hide-details
        clearable
    ></v-text-field>

    <v-list class="pt-0" flat>
      <v-list-item
          v-for="task in sortedTasks"
          :key="task.id"
          @click="doneTask(task.id)"
          :class="{'blue lighten-5': task.done}"
      >
        <v-list-item-action>
          <v-checkbox :input-value="task.done" color="primary"></v-checkbox>
        </v-list-item-action>

        <v-list-item-content>
          <div class="task-details">
            <v-list-item-title :class="{'text-decoration-line-through': task.done}">
              {{ task.title }}
            </v-list-item-title>
            <v-list-item-subtitle class="text-right" v-if="task.dueDate">
              <v-icon class="mr-2">mdi-calendar</v-icon>{{ task.dueDate }}
            </v-list-item-subtitle>
          </div>
        </v-list-item-content>

        <v-list-item-action>
          <v-menu offset-y>
            <template v-slot:activator="{ on }">
              <v-btn v-on="on" icon>
                <v-icon color="primary lighten-1">mdi-dots-vertical</v-icon>
              </v-btn>
            </template>

            <v-list>
              <v-list-item @click="editTask(task.id)">
                <v-list-item-icon>
                  <v-icon color="primary lighten-1">mdi-pencil</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  Edit
                </v-list-item-content>
              </v-list-item>

              <v-list-item @click="openDueDateDialog(task.id)">
                <v-list-item-icon>
                  <v-icon color="primary lighten-1">mdi-calendar-clock</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  Due Date
                </v-list-item-content>
              </v-list-item>

              <v-list-item @click="deleteTask(task.id)">
                <v-list-item-icon>
                  <v-icon color="primary lighten-1">mdi-delete</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  Delete
                </v-list-item-content>
              </v-list-item>

              <v-list-item @click="openSortTasksDialog">
                <v-list-item-icon>
                  <v-icon color="primary lighten-1">mdi-sort</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  Sort
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-menu>
        </v-list-item-action>
      </v-list-item>
      <v-divider></v-divider>
    </v-list>

    <!-- Due Date Dialog -->
    <v-dialog v-model="showDueDateDialog" max-width="290">
      <v-card>
        <v-card-title class="headline">Select Due Date</v-card-title>
        <v-card-text>
          <v-date-picker v-model="selectedDueDate" scrollable></v-date-picker>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="saveDueDate">Save</v-btn>
          <v-btn color="secondary" @click="closeDueDateDialog">Cancel</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Edit Task Dialog -->
    <v-dialog v-model="showEditTaskDialog" max-width="290">
      <v-card>
        <v-card-title class="headline">Edit Task</v-card-title>
        <v-card-text>
          <v-text-field
              v-model="editedTaskTitle"
              outlined
              label="Edit Task"
          ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="saveEditedTask">Save</v-btn>
          <v-btn color="secondary" @click="closeEditTaskDialog">Cancel</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Sort Tasks Dialog -->
    <v-dialog v-model="showSortTasksDialog" max-width="290">
      <v-card>
        <v-card-title class="headline">Sort Tasks</v-card-title>
        <v-card-text>
          <v-radio-group v-model="sortAscending">
            <v-radio :label="ascendingLabel" :value="true"></v-radio>
            <v-radio :label="descendingLabel" :value="false"></v-radio>
          </v-radio-group>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="sortTasks">Save</v-btn>
          <v-btn color="secondary" @click="closeSortTasksDialog">Cancel</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      newTaskTitle: '',
      tasks: [
        { id: 1, title: 'Wake up', done: false, dueDate: null },
        { id: 2, title: 'Get Bananas', done: false, dueDate: null },
        { id: 3, title: 'Eat Bananas', done: false, dueDate: null },
        // Add more tasks here...
      ],
      showDueDateDialog: false,
      selectedDueDate: null,
      taskIdForDueDate: null,
      showEditTaskDialog: false,
      editedTaskTitle: '',
      editedTaskId: null,
      showSortTasksDialog: false,
      sortAscending: true, // Default sorting direction is ascending
    };
  },
  computed: {
    ascendingLabel() {
      return 'Ascending';
    },
    descendingLabel() {
      return 'Descending';
    },
    sortedTasks() {
      if (this.sortAscending) {
        return this.tasks.slice().sort((a, b) => a.title.localeCompare(b.title));
      } else {
        return this.tasks.slice().sort((a, b) => b.title.localeCompare(a.title));
      }
    },
  },
  methods: {
    addTask() {
      let newTask = {
        id: Date.now(),
        title: this.newTaskTitle,
        done: false,
        dueDate: null,
      };
      this.tasks.push(newTask);
      this.newTaskTitle = '';
    },
    doneTask(id) {
      let task = this.tasks.find((task) => task.id === id);
      if (task) {
        task.done = !task.done;
      }
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
    openDueDateDialog(taskId) {
      this.taskIdForDueDate = taskId;
      this.selectedDueDate = this.tasks.find((task) => task.id === taskId)?.dueDate || null;
      this.showDueDateDialog = true;
    },
    closeDueDateDialog() {
      this.showDueDateDialog = false;
    },
    saveDueDate() {
      const task = this.tasks.find((task) => task.id === this.taskIdForDueDate);
      if (task) {
        task.dueDate = this.selectedDueDate;
      }
      this.showDueDateDialog = false;
    },
    editTask(taskId) {
      const task = this.tasks.find((task) => task.id === taskId);
      if (task) {
        this.editedTaskId = task.id;
        this.editedTaskTitle = task.title;
        this.showEditTaskDialog = true;
      }
    },
    closeEditTaskDialog() {
      this.showEditTaskDialog = false;
    },
    saveEditedTask() {
      const task = this.tasks.find((task) => task.id === this.editedTaskId);
      if (task) {
        task.title = this.editedTaskTitle;
      }
      this.showEditTaskDialog = false;
    },
    openSortTasksDialog() {
      this.showSortTasksDialog = true;
    },
    closeSortTasksDialog() {
      this.showSortTasksDialog = false;
    },
    sortTasks() {
      this.showSortTasksDialog = false; // Close the sort dialog
      if (this.sortAscending) {
        this.tasks.sort((a, b) => a.title.localeCompare(b.title));
      } else {
        this.tasks.sort((a, b) => b.title.localeCompare(a.title));
      }
    },
  },
};
</script>

<style>
/* Add any custom styles here */
.text-right {
  text-align: right;
  margin-bottom: -4px;
}

.task-details {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
