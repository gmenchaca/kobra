<script setup>
import { ref, computed } from 'vue';
import Button from 'primevue/button';
import Checkbox from 'primevue/checkbox';

const checkedNames = ref([]); 
const name = ref('');
const email = ref('');
const phone = ref('');
const submitted = ref(false);

const formIsValid = computed(() => {
  return name.value.trim() !== '' && email.value.trim() !== '' && phone.value.trim() !== '';
});

const handleButtonClick = () => {
  submitForm();
};

const submitForm = () => {
  submitted.value = true;

  // Creamos el FormData para la petición AJAX
  const formData = new FormData();
  
  // OBLIGATORIO: Debe coincidir con el atributo name del formulario registrado en Netlify
  formData.append('form-name', 'kobra-cotizacion');
  
  formData.append('name', name.value);
  formData.append('email', email.value);
  formData.append('phone', phone.value);
  formData.append('opciones', checkedNames.value.join(', '));   

  // Envío AJAX usando Fetch API
  fetch('/', {
    method: 'POST',
    // Importante: No definir 'Content-Type', el navegador lo establece automáticamente para FormData
    body: formData,
  })
  .then((response) => {
    if (response.ok) {
      // Redirección exitosa a la URL solicitada
      window.location.href = '/cotizacion/';
    } else {
      submitted.value = false;
      alert('Hubo un problema al enviar la cotización. Intenta de nuevo.');
    }
  })
  .catch((error) => {
    console.error('Error al enviar:', error);
    submitted.value = false;
  });
};
</script>

<template>
  <div class="surface flex justify-content-center" style="border-radius: 5px; color: white;">
    <section id="highlights" style="max-width:1140px; margin:50px auto">
      <div class="text-center">
        <h2 class="text-900 font-normal mb-2 text-primary">Obtenga una cotización en 5 minutos</h2>
      </div>
      
      <div class="card" style="text-align: left">
        <div class="grid">
          <!-- Columna Izquierda: Incluimos siempre -->
          <div class="col-12 md:col-7">
            <div class="grid">
              <div class="col-12"><h5>Lo que incluimos siempre:</h5></div>
              <div class="col-6 md:col-6"><i class="pi pi-fw pi-check text-xl text-cyan-500 mr-2"></i>1 año de Hosting</div>
              <div class="col-6 md:col-6"><i class="pi pi-fw pi-check text-xl text-cyan-500 mr-2"></i>Menú, Privacidad y Términos</div>
              <div class="col-6 md:col-6"><i class="pi pi-fw pi-check text-xl text-cyan-500 mr-2"></i>Imágenes de Stock</div>
              <div class="col-6 md:col-6"><i class="pi pi-fw pi-check text-xl text-cyan-500 mr-2"></i>Adaptable en los móviles</div>
              <div class="col-6 md:col-6"><i class="pi pi-fw pi-check text-xl text-cyan-500 mr-2"></i>Configuración Antihackeo</div>
              <div class="col-6 md:col-6"><i class="pi pi-fw pi-check text-xl text-cyan-500 mr-2"></i>Respaldo Automático</div>
              <div class="col-6 md:col-6"><i class="pi pi-fw pi-check text-xl text-cyan-500 mr-2"></i>Integración con Redes Sociales</div>
              <div class="col-6 md:col-6"><i class="pi pi-fw pi-check text-xl text-cyan-500 mr-2"></i>2 a 3 opciones de diseño</div>
              <div class="col-6 md:col-6"><i class="pi pi-fw pi-check text-xl text-cyan-500 mr-2"></i>Whatsapp o chat widget</div>
            </div>
          </div>

          <!-- Columna Derecha: Formulario y Opciones -->
          <div class="col-12 md:col-5">
            <div class="grid">
              <div class="col-12"><h5>Opciones:</h5></div>
              
              <div class="col-6 md:col-4">
                <div class="field-checkbox mb-0">
                  <Checkbox id="checkOption1" value="Promociones" v-model="checkedNames" />
                  <label for="checkOption1">Promociones</label>
                </div>
              </div>
              <div class="col-6 md:col-4">
                <div class="field-checkbox mb-0">
                  <Checkbox id="checkOption2" value="5 servicios" v-model="checkedNames" />
                  <label for="checkOption2">5 servicios</label>
                </div>
              </div>
              <div class="col-6 md:col-4">
                <div class="field-checkbox mb-0">
                  <Checkbox id="checkOption3" value="Google maps" v-model="checkedNames" />
                  <label for="checkOption3">Google maps</label>
                </div>
              </div>
              <div class="col-6 md:col-4">
                <div class="field-checkbox mb-0">
                  <Checkbox id="checkOption4" value="Catálogo online" v-model="checkedNames" />
                  <label for="checkOption4">Catálogo online</label>
                </div>
              </div>
              <div class="col-6 md:col-4">
                <div class="field-checkbox mb-0">
                  <Checkbox id="checkOption5" value="Pagos online" v-model="checkedNames" />
                  <label for="checkOption5">Pagos online</label>
                </div>
              </div>
              <div class="col-6 md:col-4">
                <div class="field-checkbox mb-0">
                  <Checkbox id="checkOption6" value="Google ads" v-model="checkedNames" />
                  <label for="checkOption6">Google ads</label>
                </div>
              </div>
            </div>

            <!-- Formulario Reactivo -->
            <form name="kobra-cotizacion" data-netlify="true" @submit.prevent="handleButtonClick">
              <div class="card p-fluid style-left" style="text-align:left">
                <div class="field">
                  <label for="name">Nombre</label>
                  <input v-model="name" id="name" name="name" class="p-inputtext p-component" type="text" required>
                </div>
                <div class="field">
                  <label for="phone">Teléfono</label>
                  <input v-model="phone" id="phone" name="phone" class="p-inputtext p-component" type="tel" required>
                </div>
                <div class="field">
                  <label for="email">E-mail</label>
                  <input v-model="email" id="email" name="email" class="p-inputtext p-component" type="email" required>
                </div>
              </div>

              <Button v-if="submitted" label="Enviando..." class="p-button-success mr-2" disabled />
              <Button v-else type="submit" :disabled="!formIsValid">Cotizar</Button>
            </form>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>