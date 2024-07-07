<template>
  <v-container>
    <v-row justify="center">
      <v-col md="8">
        <v-card style="background-color: #ECEFF1; margin-top: 20px; border-radius: 15px;">
          <v-card-title>
            <h1>Lista de Tarefas</h1>
          </v-card-title>
          <v-card-text>
            <v-btn style="margin-bottom: 30px; margin-top: 10px; border-radius: 7px;" color="primary" @click="showAddTaskDialog">
              <v-icon>mdi-plus</v-icon>
              Adicionar Tarefa
            </v-btn>
            <!-- Cabeçalhos das colunas -->
            <v-row v-if="tasks.length > 0" class="column-headers ">
              <v-col cols="12" sm="3">
                <strong>Nome</strong>
              </v-col>
              <v-col cols="12" sm="3">
                <strong>Data</strong>
              </v-col>
              <v-col cols="12" sm="3">
                <strong>Prioridade</strong>
              </v-col>
              <v-col cols="12" sm="3">
                <strong>Descrição</strong>
              </v-col>
            </v-row>
            <!-- Lista de tarefas -->
            <v-list v-if="tasks.length > 0" style="background-color: #ECEFF1;">
              <v-list-item v-for="(task, index) in tasks" :key="index" class="task-item">
                <v-list-item-content>
                  <v-list-item-title>
                    <v-row>
                      <v-col cols="12" sm="3">
                        <strong>{{ task.name }}</strong>
                      </v-col>
                      <v-col cols="12" sm="3">
                        {{ task.date }} - {{ task.time }}
                      </v-col>
                      <v-col cols="12" sm="3">
                        <v-chip :color="priorityColor(task.priority)" style="font-weight: bold;">
                          {{ task.priority }}
                        </v-chip>
                      </v-col>
                      <v-col cols="12" sm="3">
                        {{ task.description }}
                      </v-col>
                    </v-row>
                  </v-list-item-title>
                </v-list-item-content>
                <v-list-item-action>
                  <button id="botaoEditarTarefa" @click="editTask(index)">
                    <v-icon>mdi-pencil</v-icon>
                  </button>
                  <button id="botaoExcluirTarefa" @click="deleteTask(index)">
                    <v-icon>mdi-delete</v-icon>
                  </button>
                </v-list-item-action>
              </v-list-item>
            </v-list>
            <p v-else>
              Não há tarefas.
            </p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Dialog para adicionar tarefa -->
    <v-dialog v-model="addTaskDialog" max-width="600px">
      <v-card color="teal-darken-2">
        <v-card-title style="text-align: center;">
          <h2>Criar Tarefa</h2>
        </v-card-title>
        <v-card-text>
          <v-form @submit.prevent="addTask">
            <v-text-field v-model="newTask.name" label="Nome da Tarefa" required></v-text-field>
            <v-row>
              <v-col cols="12" sm="6">
                <v-text-field v-model="newTask.date" label="Data" type="date" required></v-text-field>
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field v-model="newTask.time" label="Hora" ref="timePicker" type="time" @click="openTimeSelector" required></v-text-field>
              </v-col>
            </v-row>
            <v-select v-model="newTask.priority" :items="priorities" label="Nível de Importância" required></v-select>
            <v-textarea v-model="newTask.description" label="Descrição" required></v-textarea>
            <v-btn type="submit" color="primary">
              <v-icon class="mr-1">mdi-plus</v-icon>
              Adicionar
            </v-btn>

            <v-btn @click="cancelAddTask" color="red" class="ml-4">
              <v-icon class="mr-2">mdi-cancel</v-icon>
              Cancelar
            </v-btn>

          </v-form>
        </v-card-text>
      </v-card>
    </v-dialog>

    <!-- Dialog para editar tarefa -->
    <v-dialog v-model="editTaskDialog" max-width="600px">
      <v-card color="teal-darken-2">
        <v-card-title>
          <span>Editar Tarefa</span>
        </v-card-title>
        <v-card-text>
          <v-form @submit.prevent="updateTask">
            <v-text-field v-model="editedTask.name" label="Nome da Tarefa" required></v-text-field>
            <v-row>
              <v-col cols="12" sm="6">
                <v-text-field v-model="editedTask.date" label="Data" ref="editDatePicker" type="date" required></v-text-field>
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field v-model="editedTask.time" label="Hora" ref="editTimePicker" type="time" required></v-text-field>
              </v-col>
            </v-row>
            <v-select v-model="editedTask.priority" :items="priorities" label="Nível de Importância" required></v-select>
            <v-textarea v-model="editedTask.description" label="Descrição" required></v-textarea>
            <v-btn type="submit" color="primary">Salvar</v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      addTaskDialog: false,
      editTaskDialog: false,
      newTask: {
        name: '',
        date: null,
        time: null,
        priority: null,
        description: ''
      },
      editedTask: {
        name: '',
        date: null,
        time: null,
        priority: null,
        description: ''
      },
      editedTaskIndex: -1,
      priorities: ['Baixa', 'Média', 'Alta']
    };
  },
  methods: {
    showAddTaskDialog() {
      this.addTaskDialog = true;
    },
    addTask() {
      if (this.newTask.date && this.newTask.time) {
        this.tasks.push({ ...this.newTask });
        this.resetNewTask();
        this.addTaskDialog = false;
      }
    },

    cancelAddTask() {
      this.resetNewTask();
      this.addTaskDialog = false;
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.editedTask = { ...this.tasks[index] };
      this.editedTaskIndex = index;
      this.editTaskDialog = true;
    },
    updateTask() {
      if (this.editedTask.date && this.editedTask.time) {
        this.tasks.splice(this.editedTaskIndex, 1, { ...this.editedTask });
        this.resetEditedTask();
        this.editTaskDialog = false;
      }
    },
    resetNewTask() {
      this.newTask = {
        name: '',
        date: null,
        time: null,
        priority: null,
        description: ''
      };
    },
    resetEditedTask() {
      this.editedTask = {
        name: '',
        date: null,
        time: null,
        priority: null,
        description: ''
      };
    },
    priorityColor(priority) {
      switch (priority) {
        case 'Baixa':
          return 'green';
        case 'Média':
          return 'orange';
        case 'Alta':
          return 'red';
      }
    },
    openDateSelector() {
      this.$refs.datePicker.click();
    },
    openTimeSelector() {
      this.$refs.timePicker.click();
    }
  }
};
</script>

<style scoped>
.v-list-item-action {
  margin-bottom: 15px;
}

.v-list-item {
  margin-top: 5px;
  margin-bottom: 20px;
  background-color: #C8E6C9;
  border-radius: 10px !important;
}

#botaoEditarTarefa {
  font-size: 15px;
  margin-top: 7px;
}

#botaoExcluirTarefa {
  font-size: 15px;
  margin-left: 30px;
  margin-top: 7px;
}

.column-headers strong {
  font-size: 16px;
}
</style>
