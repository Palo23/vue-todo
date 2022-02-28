<template>
  <div class="container">
    <h2 class="text-center mt-5">Lista de tareas</h2>

    <!-- Card to submit task -->
    <div class="row">
      <div class="col-12 d-flex justify-content-center">
        <div class="card padding-card col-lg-6 col-md-12 col-sm-12 col-xs-12">
          <div class="card-body">
              <div class="form-group mt-2">
                <input v-model="title" type="text" class="form-control" name="title" placeholder="Título">
              </div>

              <div class="form-group mt-2">
                <input v-model="attendant" type="text" class="form-control" name="attendant" placeholder="Responsable">
              </div>

              <div class="form-group mt-2">
                <input v-model="description" type="text" class="form-control" name="description" placeholder="Descripción">
              </div>

              <div class="form-group mt-2">
                <select v-model="priority" name="priority" id="priority" class="form-control">
                  <option value="">Select Priority</option>
                  <option value="low">Low</option>
                  <option value="medium">Medium</option>
                  <option value="high">High</option>
                </select>
              </div>

              <div class="form-group d-flex justify-content-center mt-2">
                <button @click="submitTask" type="submit" class="btn btn-primary">Agregar</button>
              </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Card to show tasks -->

    <div class="row mt-5 mb-5">
      <div v-for="(task, index) in tasks" :key="index" class="col-lg-4 col-md-6 col-sm-12">
        <div class="mt-2 card col-12">
          <div class="card-header flex-column d-flex justify-content-center align-items-center">
                <p>{{task.title}}</p>
                <span 
                class="badge badge-pill" 
                :class="{ 
                  'badge-danger': task.priority === 'high',
                  'badge-medium': task.priority === 'medium',
                  'badge-primary': task.priority === 'low' 
                }"
                >{{task.priority}}</span>
          </div>

          <div class="card-body">
            <div class="body-card flex-column d-flex justify-content-center align-items-center">
                <p>{{task.description}}</p>
                <span class="badge badge-primary">{{task.attendant}}</span>
            </div>
          </div>

          <div class="card-footer d-flex justify-content-center">
              <button class="btn btn-danger" @click="deleteTask(index)">Eliminar</button>
            </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import Swal from 'sweetalert2/dist/sweetalert2.js'

export default {
  name: 'ToDo',
  props: {
    msg: String
  },

  data() {
    return {
      title: '',
      priority: '',
      attendant: '',
      description: '',
      tasks: []
    }
  },

  methods: {
    submitTask() {

      if(this.title === '' || this.priority === '' || this.description === '' || this.attendant === '') {
        Swal.fire('Debes ingresar todos los campos', '', 'error');
        return;
      }

      let taskObject = {
        title: this.title,
        priority: this.priority,
        description: this.description,
        attendant: this.attendant
      }

      this.tasks.push(taskObject);

      localStorage.setItem('tasks', JSON.stringify(this.tasks));

      this.title = '';
      this.priority = '';
      this.description = '';
      this.attendant = '';
    },

    deleteTask(index) {

      Swal.fire({
        title: '¿Seguro que deseas eliminar?',
        showCancelButton: true,
        confirmButtonText: 'Sí',
        cancelButtonText: 'No',
      }).then((result) => {
        if (result.isConfirmed) {
          let newTasks = this.tasks.splice(index, 1);
          localStorage.setItem('tasks', JSON.stringify(newTasks));
          Swal.fire('¡Eliminado!', '', 'success')
        } 
      });
    }
  },
  mounted() {
    if (localStorage.getItem('tasks')) {
      this.tasks = JSON.parse(localStorage.getItem('tasks'));
    }
  }
}
</script>

<style>
  .padding-card {
    border-radius: 8px;
    padding: 1rem;
  }

  .header-card {
    border-bottom: 1px solid black;
    background-color: #f7f7f7;
  }

  .footer-card {
    border-top: 1px solid black;
    background-color: #f7f7f7;
  }

  .badge-danger {
    background-color: #dc3545;
  }

  .badge-primary {
    background-color: #007bff;
  }

  .badge-medium {
    background-color: #6c757d;
  }

  @media screen and (max-width: 575px) {
    .col-xs-12 {
      width: 100%;
    }
  }
</style>
