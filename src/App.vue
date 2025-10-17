<template>
  <div class="container mt-5">
    <h2 class="text-center mb-4">Registro de Estudiantes</h2>

    <form @submit.prevent="agregarEstudiante">
      <div class="mb-3">
        <label>Nombre:</label>
        <input type="text" v-model="nuevo.nombre" class="form-control" required>
      </div>

      <div class="mb-3">
        <label>Curso:</label>
        <input type="text" v-model="nuevo.curso" class="form-control" required>
      </div>

      <button type="submit" class="btn btn-primary">Agregar</button>
    </form>

    <hr>

    <h3>Estudiantes Registrados</h3>
    <ul class="list-group">
      <li class="list-group-item" v-for="est in estudiantes" :key="est.id">
        {{ est.nombre }} - {{ est.curso }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      estudiantes: [],
      nuevo: { nombre: '', curso: '' }
    };
  },
  methods: {
    async obtenerEstudiantes() {
      try {
        const res = await axios.get('http://localhost:3000/estudiantes');
        this.estudiantes = res.data.estudiantes; // <- aquí usamos res.data.estudiantes según tu backend
      } catch (error) {
        console.error("Error al obtener estudiantes:", error);
      }
    },
    async agregarEstudiante() {
      try {
        await axios.post('http://localhost:3000/estudiantes', this.nuevo);
        this.nuevo = { nombre: '', curso: '' };
        this.obtenerEstudiantes();
      } catch (error) {
        console.error("Error al agregar estudiante:", error);
      }
    }
  },
  mounted() {
    this.obtenerEstudiantes();
  }
}
</script>
