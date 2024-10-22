<template>
  <div class="container mt-5">
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

    <!-- Modal para Agendar Evento -->
    <b-modal v-model="showModal" title="Agendar Evento" @hide="resetForm">
      <template #modal-footer>
        <b-button variant="secondary" @click="showModal = false">Cerrar</b-button>
        <b-button variant="primary" @click="submitEvent">Agendar</b-button>
      </template>

      <div class="mb-3">
        <label for="modal-name" class="form-label">Nombre del Evento</label>
        <input 
          type="text" 
          v-model="form.name" 
          class="form-control" 
          id="modal-name" 
          placeholder="Ingrese el nombre del evento" 
          required 
          aria-label="Nombre del evento"
        />
      </div>
      <div class="mb-3">
        <label for="modal-date" class="form-label">Fecha</label>
        <input 
          type="date" 
          v-model="form.date" 
          class="form-control" 
          id="modal-date" 
          disabled 
          aria-label="Fecha del evento seleccionada"
        />
      </div>
      <div class="mb-3">
        <label for="modal-time" class="form-label">Hora</label>
        <input 
          type="time" 
          v-model="form.time" 
          class="form-control" 
          id="modal-time" 
          required 
          aria-label="Hora del evento"
        />
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

    const submitEvent = () => {
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
      submitEvent,
      resetForm
    };
  }
}
</script>

<style scoped>
.container {
  max-width: 900px;
  margin: 0 auto;
}
</style>
