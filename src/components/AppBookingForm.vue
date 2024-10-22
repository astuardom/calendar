<template>
  <div class="container mt-5">
    <!-- Alerta de éxito -->
    <div v-if="alertVisible" class="alert alert-success" role="alert">
      Visita agendada con éxito.
    </div>

    <div class="card">
      <div class="card-header bg-success text-white">
        <h3>Formulario de Agendamiento</h3>
      </div>
      <div class="card-body">
        <form @submit.prevent="handleSubmit">
          <div class="mb-3">
            <label for="name" class="form-label">Nombre</label>
            <input type="text" v-model="form.name" class="form-control" id="name" placeholder="Ingrese su nombre" required />
          </div>
          <div class="mb-3">
            <label for="date" class="form-label">Fecha</label>
            <input type="date" v-model="form.date" class="form-control" id="date" required />
          </div>
          <div class="mb-3">
            <label for="time" class="form-label">Hora</label>
            <input type="time" v-model="form.time" class="form-control" id="time" required />
          </div>
          <button type="submit" class="btn btn-primary" :disabled="!isFormValid">Agendar Visita</button>
        </form>
      </div>
    </div>

    <!-- Modal de Confirmación -->
    <b-modal v-model="showModal" title="Confirmar Agendamiento" @hide="resetForm">
      <template #modal-footer>
        <b-button variant="secondary" @click="showModal = false">Cancelar</b-button>
        <b-button variant="primary" @click="confirmAgendamento">Confirmar</b-button>
      </template>

      <p>¿Está seguro de que desea agendar la visita para:</p>
      <p><strong>Nombre:</strong> {{ form.name }}</p>
      <p><strong>Fecha:</strong> {{ form.date }}</p>
      <p><strong>Hora:</strong> {{ form.time }}</p>
    </b-modal>
  </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  setup() {
    const form = ref({
      name: '',
      date: '',
      time: ''
    });
    const showModal = ref(false);
    const alertVisible = ref(false);

    // Validación para asegurarse de que los campos están llenos y la fecha/hora es válida
    const isFormValid = computed(() => {
      const now = new Date();
      const selectedDate = new Date(`${form.value.date}T${form.value.time}`);
      return form.value.name && selectedDate > now;
    });

    const handleSubmit = () => {
      // Muestra el modal para confirmar la agendación
      showModal.value = true;
    };

    const confirmAgendamento = () => {
      // Lógica para enviar los datos o realizar una acción tras confirmar
      console.log('Agendado:', form.value);
      alertVisible.value = true;
      setTimeout(() => alertVisible.value = false, 3000); // Ocultar alerta después de 3 segundos
      resetForm();
      showModal.value = false; // Cerrar el modal
    };

    const resetForm = () => {
      form.value.name = '';
      form.value.date = '';
      form.value.time = '';
    };

    return {
      form,
      showModal,
      alertVisible,
      isFormValid,
      handleSubmit,
      confirmAgendamento,
      resetForm
    };
  }
}
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 20px auto;
  padding: 15px;
}
.card {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
</style>
