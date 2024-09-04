<template>
  <div class="app-container">
    <nav class="navbar navbar-light bg-light">
      <a class="navbar-brand"
        >⚕️ Urgencias Hospital Santa Carolina de los Denunciados ⚕️</a
      >
    </nav>

    <form @submit.prevent="submitForm">
      <div>
        <label :class="{ 'error-label': !form.paciente }">Paciente</label>
        <input type="text" v-model="form.paciente" />
      </div>

      <div>
        <label :class="{ 'error-label': !form.fecha }">Fecha</label>
        <input type="date" v-model="form.fecha" />
      </div>

      <div>
        <label :class="{ 'error-label': !form.hora }">Hora</label>
        <input type="time" v-model="form.hora" />
      </div>

      <div>
        <label :class="{ 'error-label': !form.gravedad }">Gravedad</label>
        <select v-model="form.gravedad">
          <option value="">Seleccione una opción</option>
          <option value="Baja">Baja</option>
          <option value="Media">Media</option>
          <option value="Alta">Alta</option>
        </select>
      </div>

      <div>
        <label :class="{ 'error-label': !form.motivo }">Motivo</label>
        <input type="text" v-model="form.motivo" />
      </div>

      <button type="submit">Agregar</button>
    </form>

    <p v-if="citas.length === 0" class="no-citas">
      Aún no hay consultas registradas
    </p>

    <!-- renderizanding -->
    <div class="cita-container">
      <div
        v-for="(cita, index) in sortedCitas"
        :key="index"
        :class="['cita-card', cita.gravedad.toLowerCase()]"
      >
        <h3>{{ cita.paciente }}</h3>
        <p><strong>Fecha:</strong> {{ cita.fecha }}</p>
        <p><strong>Hora:</strong> {{ cita.hora }}</p>
        <p><strong>Motivo:</strong> {{ cita.motivo }}</p>
        <button @click="eliminarCita(index)">Eliminar</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        paciente: "",
        fecha: "",
        hora: "",
        gravedad: "",
        motivo: "",
      },
      citas: [], // guardar la citas
    };
  },
  methods: {
    submitForm() {
      if (this.validarFormulario()) {
        // agregar la cita a la lista
        this.citas.push({ ...this.form });

        // limpiar el formulario
        this.form = {
          paciente: "",
          fecha: "",
          hora: "",
          gravedad: "",
          motivo: "",
        };
      }
    },
    validarFormulario() {
      return (
        this.form.paciente &&
        this.form.fecha &&
        this.form.hora &&
        this.form.gravedad &&
        this.form.motivo
      );
    },

    eliminarCita(index) {
      this.citas.splice(index, 1);
    },
  },

  //esta parte es experimental para que las citas se ordenen de mayor a menor gravedad,
  //creo que podría ser más eficiente de otra manera pero es la única que se me ocurrió por ahora

  computed: {
    sortedCitas() {
      const gravedadValores = { Alta: 3, Media: 2, Baja: 1 };
      return [...this.citas].sort((a, b) => {
        return gravedadValores[b.gravedad] - gravedadValores[a.gravedad];
      });
    },
  },
};
</script>

<style scoped>
/* fondorgh */
.app-container {
  background-color: #e0f7fa;
  padding: 20px;
  min-height: 100vh;
}

.navbar {
  border-bottom: 2px solid rgb(147, 255, 241);
  justify-content: center;
}

.navbar-brand {
  font-size: 30px;
  font-weight: bold;
}

/* form */
form {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  margin: 0 auto 20px;
  margin-top: 30px;
  display: flex;
  flex-direction: column;
}

label {
  font-weight: bold;
  color: var(--text-color);
  margin-bottom: 5px;
}

input,
select {
  width: 100%;
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid var(--secondary-color);
  border-radius: 5px;
  box-sizing: border-box;
}

input[type="text"] {
  background-color: #eaeaeae3;
}

/* botoncito */
button[type="submit"] {
  background-color: rgb(147, 255, 241);
  color: rgb(0, 0, 0);
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

button[type="submit"]:hover {
  background-color: darken(var(--primary-color), 10%);
}

/* error */
.error-label {
  color: var(--error-color);
}

/* no hay citas*/
.no-citas {
  text-align: center;
  color: var(--error-color);
  font-weight: bold;
}

/* container citas */
.cita-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}

/* card cita */
.cita-card {
  flex: 1 1 300px;
  max-width: 300px;
  border: 1px solid var(--secondary-color);
  padding: 16px;
  margin-top: 10px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.baja {
  background-color: #d4edda;
  color: #155724;
}

.media {
  background-color: #fff3cd;
  color: #856404;
}

.alta {
  background-color: #f8d7da;
  color: #721c24;
}
</style>
