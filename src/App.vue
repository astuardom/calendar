<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-6">
        <div class="card">
          <div class="card-header bg-success text-white">
            <h3>Formulario de Agendamiento</h3>
          </div>
          <div class="card-body">
            <form @submit.prevent="handleMainFormSubmit">
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
              <button type="submit" class="btn btn-primary">Agendar Visita</button>
            </form>
          </div>
        </div>
      </div>
      <div class="col-md-6">
        <div class="card">
          <div class="card-header bg-primary text-white">
            <h3>Calendario de Visitas</h3>
          </div>
          <div class="card-body">
            <FullCalendar
              :plugins="[dayGridPlugin, timeGridPlugin]"
              :headerToolbar="{
                left: 'prev,next today',
                center: 'title',
                right: 'dayGridMonth,timeGridWeek,timeGridDay'
              }"
              :events="events"
              @dateClick="handleDateClick"
            />
          </div>
        </div>
      </div>
    </div>

    <!-- Modal para Agendar Visita -->
    <b-modal v-model="showModal" title="Agendar Visita" @hide="resetForm">
      <template #modal-footer>
        <b-button variant="secondary" @click="showModal = false">Cerrar</b-button>
        <b-button variant="primary" @click="submitForm">Agendar</b-button>
      </template>

      <div class="mb-3">
        <label for="modal-name" class="form-label">Nombre</label>
        <input type="text" v-model="form.name" class="form-control" id="modal-name" placeholder="Ingrese su nombre" required />
      </div>
      <div class="mb-3">
        <label for="modal-date" class="form-label">Fecha</label>
        <input type="date" v-model="form.date" class="form-control" id="modal-date" disabled />
      </div>
      <div class="mb-3">
        <label for="modal-time" class="form-label">Hora</label>
        <input type="time" v-model="form.time" class="form-control" id="modal-time" required />
      </div>
    </b-modal>
  </div>
</template>

<script>
import FullCalendar from '@fullcalendar/vue3';
import dayGridPlugin from '@fullcalendar/daygrid';
import timeGridPlugin from '@fullcalendar/timegrid';
import { ref } from 'vue';

export default {
  components: {
    FullCalendar
  },
  setup() {
    const showModal = ref(false);
    const form = ref({
      name: '',
      date: '',
      time: ''
    });
    const events = ref([]);

    const handleDateClick = (arg) => {
      form.value.date = arg.dateStr; // Guardar la fecha seleccionada
      showModal.value = true; // Mostrar el modal
    };

    const handleMainFormSubmit = () => {
      // Solo se usa para el formulario principal, si decides usarlo
      console.log("Formulario principal enviado");
    };

    const submitForm = () => {
      if (form.value.name && form.value.date && form.value.time) {
        events.value.push({
          title: form.value.name,
          start: `${form.value.date}T${form.value.time}:00`
        });
        resetForm();
        showModal.value = false; // Cerrar el modal
      }
    };

    const resetForm = () => {
      form.value.name = '';
      form.value.date = '';
      form.value.time = '';
    };

    return {
      dayGridPlugin,
      timeGridPlugin,
      showModal,
      form,
      events,
      handleDateClick,
      submitForm,
      resetForm,
      handleMainFormSubmit
    };
  }
}
</script>

<style scoped>
.container {
  max-width: 900px;
  margin: 0 auto;
}
.card {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
</style>
