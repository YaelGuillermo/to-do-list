<template>
    <div class="p-4"> <!-- Agregamos padding al contenedor principal -->
      <h2 class="text-2xl text-gray-700 mb-4">Todo Registration</h2> <!-- Tamaño del texto y margen inferior -->
      <form @submit.prevent="saveTodo">
        <div class="mb-4"> <!-- Margen inferior -->
          <label for="activity" class="block text-xs font-medium text-gray-700 mb-1">Activity</label> <!-- Margen inferior para el texto y para el input -->
          <input
            type="text"
            v-model="activity"
            id="activity"
            placeholder="Activity"
            class="w-full rounded-md border-gray-200 shadow-sm sm:text-sm focus:ring-indigo-500 focus:border-indigo-500" 
            required
          />
        </div>
        <div class="mb-4"> <!-- Margen inferior -->
          <label for="startdate" class="block text-xs font-medium text-gray-700 mb-1">Start date</label> <!-- Margen inferior para el texto y para el input -->
          <input type="date" v-model="startdate" id="startdate" class="w-full rounded-md border-gray-200 shadow-sm sm:text-sm focus:ring-indigo-500 focus:border-indigo-500" required /> <!-- Clases para el ancho, bordes, sombra y estilo de foco -->
        </div>
        <div class="mb-4"> <!-- Margen inferior -->
          <label for="finaldate" class="block text-xs font-medium text-gray-700 mb-1">Final date</label> <!-- Margen inferior para el texto y para el input -->
          <input type="date" v-model="finaldate" id="finaldate" class="w-full rounded-md border-gray-200 shadow-sm sm:text-sm focus:ring-indigo-500 focus:border-indigo-500" required /> <!-- Clases para el ancho, bordes, sombra y estilo de foco -->
        </div>
        
        <div>
          <button type="submit" class="bg-indigo-500 text-white px-4 py-2 rounded-md hover:bg-indigo-600">Save</button> <!-- Clases para el color de fondo, color de texto, padding y bordes redondeados, y hover -->
        </div>
      </form>
  
      <div class="mt-8"> <!-- Margen superior -->
        <h2 class="text-2xl text-gray-700 mb-4">Todos</h2> <!-- Tamaño del texto y margen inferior -->
        <table class="w-full border-collapse"> <!-- Ancho completo y colapsado de bordes -->
          <thead>
            <tr>
              <th class="text-left px-4 py-2 bg-gray-100">Activity</th> <!-- Alineación del texto a la izquierda, padding, fondo gris claro -->
              <th class="text-left px-4 py-2 bg-gray-100">Start date</th> <!-- Alineación del texto a la izquierda, padding, fondo gris claro -->
              <th class="text-left px-4 py-2 bg-gray-100">Final date</th> <!-- Alineación del texto a la izquierda, padding, fondo gris claro -->
              <th class="text-left px-4 py-2 bg-gray-100">Actions</th> <!-- Alineación del texto a la izquierda, padding, fondo gris claro -->
            </tr>
          </thead>
          <tbody>
            <tr v-for="(todo, index) in todos" :key="index" class="border-b"> <!-- Bordes inferiores -->
              <td class="px-4 py-2">
                <input type="checkbox" v-model="todo.completed" @input="editTodo(index)"/> {{ todo.activity }}
              </td>
              <td class="px-4 py-2">{{ todo.startdate }}</td> <!-- Padding -->
              <td class="px-4 py-2">{{ todo.finaldate }}</td> <!-- Padding -->
              <td class="px-4 py-2">
                <button @click="editTodo(index)" class="bg-blue-500 text-white px-2 py-1 rounded-md mr-2 hover:bg-blue-600">Edit</button> <!-- Color de fondo, color de texto, padding, bordes redondeados, margen derecho y hover -->
                <button @click="deleteTodo(index)" class="bg-red-500 text-white px-2 py-1 rounded-md hover:bg-red-600">Delete</button> <!-- Color de fondo, color de texto, padding, bordes redondeados y hover -->
              </td>
            </tr>
          </tbody>
        </table>
         <Calendar/>
      </div>
      <Calendar/>
    </div>
  </template>
  
  
  <script>
  import Calendar from '@/components/Calendar.vue'

  export default {
    data() {
    const currentDate = new Date().toISOString().substr(0, 10); // Obtener la fecha actual en formato YYYY-MM-DD
        return {
            activity: '',
            startdate: currentDate, // Establecer la fecha actual como valor por defecto para startdate
            finaldate: currentDate, // Establecer la fecha actual como valor por defecto para finaldate
            completed: false,
            todos: [],
            editingIndex: null
        };
    },
    methods: {
      saveTodo() {
        // Crear objeto de usuario
        const newTodo = {
          activity: this.activity,
          startdate: this.startdate,
          finaldate: this.finaldate,
          completed: this.completed
        };
  
        // Si se está editando un usuario, actualiza en lugar de agregar
        if (this.editingIndex !== null) {
          this.todos.splice(this.editingIndex, 1, newTodo);
          this.editingIndex = null; // Termina el modo de edición
        } else {
          // Agregar nuevo usuario a la lista
          this.todos.push(newTodo);
        }
  
        // Guardar lista de usuarios en Local Storage
        localStorage.setItem('todos', JSON.stringify(this.todos));
  
        // Limpiar campos después de guardar
        this.activity = '';
        this.startdate = '';
        this.finaldate = '';
        this.completed = false;
      },
      editTodo(index) {
        const todoToEdit = this.todos[index];
        this.activity = todoToEdit.activity;
        this.startdate = todoToEdit.startdate;
        this.finaldate = todoToEdit.finaldate;
        this.completed = todoToEdit.completed;
        this.editingIndex = index;
      },
      deleteTodo(index) {
        this.todos.splice(index, 1);
        localStorage.setItem('todos', JSON.stringify(this.todos));
      }
    },
    created() {
      // Obtener lista de usuarios del Local Storage al cargar la página
      const todosFromStorage = JSON.parse(localStorage.getItem('todos'));
      if (todosFromStorage) {
        this.todos = todosFromStorage;
      }
    }
  };
  </script>