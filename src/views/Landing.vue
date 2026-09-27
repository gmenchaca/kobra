<script setup>
import { ref, computed } from 'vue';
import Stepper from 'primevue/stepper';
import StepperPanel from 'primevue/stepperpanel';
import IconField from 'primevue/iconfield';
import InputIcon from 'primevue/inputicon'; 
import Password from 'primevue/password';   
import Button from 'primevue/button';
import ToggleButton from 'primevue/togglebutton';
import InputText from 'primevue/inputtext';
import Textarea from 'primevue/textarea';
import InputNumber from 'primevue/inputnumber';
import FloatLabel from 'primevue/floatlabel';
import RadioButton from 'primevue/radiobutton';
import Proyectos from './Proyectos.vue';

const active = ref(0);
const completed = ref(false);
const name = ref('');
const email = ref('');
const phone = ref('');
const negocio = ref('');
const hiddenInput = ref('Negocio');
const categories = ref([
  { name: 'Negocio', key: 'A' },
  { name: 'Empresa', key: 'B' },
  { name: 'Educativo', key: 'C' },
  { name: 'Otro', key: 'D' }
]); 
const submitted = ref(false);

const formIsValid = computed(() => {
  return name.value.trim() !== '' && email.value.trim() !== '' && phone.value.trim() !== '';
});

const submitForm = () => {
  submitted.value = true;

  const params = new URLSearchParams();
  // Nombre con el que se registrará este formulario en Netlify
  params.append('form-name', 'landing-stepper');
  
  params.append('name', name.value);
  params.append('email', email.value);
  params.append('phone', phone.value);
  params.append('hiddenInput', hiddenInput.value);
  params.append('negocio', negocio.value);

  fetch('/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
    body: params.toString(),
  })
  .then((response) => {
    if (response.ok) {
      window.location.href = '/cotizacion/';
    } else {
      submitted.value = false;
      alert('Hubo un problema al enviar la cotización. Intenta de nuevo.');
    }
  })
  .catch((error) => {
    console.error('Error:', error);
    submitted.value = false;
  });
};
</script>   

<template>
    <div id="home" class="landing-wrapper overflow-hidden" style="z-index: 2;">
        <div class="grid grid-nogutter text-800" style="background: transparent;max-width: 1140px;margin: 0px auto;align-items: center;text-align:center">
            <div class="col-12 md:col-8 text-center md:text-left">
                <section id="intro" style="margin-bottom:20px">
                    <h1>
                    <span class="block textkobra-6xl font-bold mb-1">Obtén una Página Web</span>
                    <div class="textkobra-6xl text-primary font-bold mb-3">Optimizada para Vender Más</div>
                    </h1>
                    <h2 class="text-900 font-normal mb-2">Desde <strong>$5,000</strong></h2>
                    <br />
                    <p class="mt-0 mb-4 line-height-3" style="font-size: 20px;">
                        <i class="pi pi-thumbs-up-fill"></i> Aumenta tus ventas y presencia digital
                    </p>
                    <p class="mt-0 mb-4 line-height-3" style="font-size: 20px;">
                        <i class="pi pi-thumbs-up-fill"></i> Entrega rápida y diseño 100% responsivo (móvil y PC)
                    </p>
                    <p class="mt-0 mb-4 line-height-3" style="font-size: 20px;">
                        <i class="pi pi-thumbs-up-fill"></i> Dominio y Soporte incluido / Listo para pauta comercial
                    </p>
                    <a href="https://wa.me/c/5215650157964" target="new_blank" style="display:none">
                        <Button label="Catálogo" type="button" class="mr-3 p-button-raised"></Button>
                    </a>
                    <a href="https://kobra-marketing.com/cotizar" target="new_blank">
                        <Button label="Cotizar" type="button" class="p-button-outlined"></Button>
                    </a>&nbsp;&nbsp; 
                    <a style="display:none" href="https://kobra-marketing.com/paquetes" target="new_blank">
                        <Button label="Paquetes"/>
                    </a>
                </section>
            </div>
            <div class="col-12 md:col-4 overflow-hidden" style="margin:0 auto">
                <div class="card p-card flex justify-content-center rainbow">
                    
                    <!-- Formulario que engloba todo el Stepper para Netlify -->
                    <form name="landing-stepper" data-netlify="true" @submit.prevent="submitForm" style="width: 100%;">
                        <Stepper v-model:activeStep="active">
                            
                            <!-- PASO 1 -->
                            <StepperPanel>
                                <template #header="{ index, clickCallback }">
                                    <Button type="button" class="bg-transparent border-none inline-flex flex-column" @click="clickCallback">
                                        <span :class="['border-round border-2 w-3rem h-3rem inline-flex align-items-center justify-content-center', { 'bg-primary border-primary': index <= active, 'surface-border': index > active }]">
                                            <i class="pi pi-briefcase" style="color: white;" />
                                        </span>
                                    </Button>
                                </template>
                                <template #content="{ nextCallback }">
                                    <div class="flex flex-column gap-2 mx-auto" style="min-height: 16rem; max-width: 20rem;">
                                        <h2>Calcula el costo de tu sitio web gratis</h2>
                                        <div class="field p-fluid">
                                            <div v-for="category in categories" :key="category.key" class="flex align-items-center mb-2">
                                                <RadioButton v-model="hiddenInput" :inputId="category.key" name="hiddenInput" :value="category.name" />
                                                <label :for="category.key" class="ml-2">{{ category.name }}</label>
                                            </div>
                                            <InputText v-model="negocio" name="negocio" placeholder="Escribe tu giro o tipo de negocio" />
                                        </div>
                                    </div>
                                    <div class="flex pt-4 justify-content-end">
                                        <Button type="button" label="Siguiente" icon="pi pi-arrow-right" iconPos="right" @click="nextCallback" />
                                    </div>
                                </template>
                            </StepperPanel>

                            <!-- PASO 2 -->
                            <StepperPanel>
                                <template #header="{ index, clickCallback }">
                                    <Button type="button" class="bg-transparent border-none inline-flex flex-column" @click="clickCallback">
                                        <span :class="['border-round border-2 w-3rem h-3rem inline-flex align-items-center justify-content-center', { 'bg-primary border-primary': index <= active, 'surface-border': index > active }]">
                                            <i class="pi pi-whatsapp" style="color: white;" />
                                        </span>
                                    </Button>
                                </template>
                                <template #content="{ prevCallback, nextCallback }">
                                    <div class="flex flex-column gap-2 mx-auto" style="min-height: 16rem; max-width: 20rem;">
                                        <h2>Tu número de teléfono con whatsapp:</h2>
                                        <div class="field p-fluid">
                                            <FloatLabel>
                                                <InputText v-model="phone" id="phone" name="phone" required type="tel" />
                                                <label for="phone">Número con whatsapp</label>
                                            </FloatLabel>
                                        </div>
                                    </div>
                                    <div class="flex pt-4 justify-content-between">
                                        <Button type="button" label="Anterior" severity="secondary" icon="pi pi-arrow-left" @click="prevCallback" />
                                        <Button type="button" label="Siguiente" icon="pi pi-arrow-right" iconPos="right" @click="nextCallback" />
                                    </div>
                                </template>
                            </StepperPanel>

                            <!-- PASO 3 -->
                            <StepperPanel>
                                <template #header="{ index, clickCallback }">
                                    <Button type="button" class="bg-transparent border-none inline-flex flex-column" @click="clickCallback">
                                        <span :class="['border-round border-2 w-3rem h-3rem inline-flex align-items-center justify-content-center', { 'bg-primary border-primary': index <= active, 'surface-border': index > active }]">
                                            <i class="pi pi-user" style="color: white;" />
                                        </span>
                                    </Button>
                                </template>
                                <template #content="{ prevCallback }">
                                    <div class="flex flex-column gap-2 mx-auto" style="min-height: 16rem; max-width: 24rem;">
                                        <h2>Datos de contacto</h2>
                                        <div class="field p-fluid">
                                            <IconField>
                                                <InputIcon>
                                                    <i class="pi pi-user" />
                                                </InputIcon>
                                                <InputText v-model="name" id="name" name="name" required type="text" placeholder="Nombre" />
                                            </IconField>
                                        </div>
                                        <div class="field p-fluid">
                                            <IconField>
                                                <InputIcon>
                                                    <i class="pi pi-envelope" />
                                                </InputIcon>
                                                <InputText v-model="email" id="email" name="email" required type="email" placeholder="Email" />
                                            </IconField>
                                        </div>
                                    </div>
                                    <div class="flex pt-4 justify-content-between">
                                        <Button type="button" label="Anterior" severity="secondary" icon="pi pi-arrow-left" @click="prevCallback" />
                                        <Button v-if="submitted" label="Enviando..." class="p-button-success mr-2" disabled />
                                        <Button v-else type="submit" label="Cotizar" icon="pi pi-arrow-up" iconPos="right" :disabled="!formIsValid" />
                                    </div>
                                </template>
                            </StepperPanel>

                        </Stepper>
                    </form>

                </div>
            </div>
        </div>
    </div>
    <Proyectos/>
</template>
  
<style scoped>
:deep(.p-stepper-header) {
  padding: 0 !important;
}

:deep(.p-stepper-content) {
  padding: 0 !important;
}

@media only screen and (max-width: 576px) {
  .rainbow { max-width: 350px; }
}

.p-stepper {
  flex-basis: 40rem;
}

@keyframes rotate {
  100% {
    transform: rotate(1turn);
  }
}

.rainbow {
  position: relative;
  z-index: 0;
  width: auto;
  height: 505px;
  border-radius: 10px;
  overflow: hidden;
  padding: 2rem;
  margin: 0 auto;
  
  &::before {
    content: '';
    position: absolute;
    z-index: -2;
    left: -50%;
    top: -50%;
    width: 200%;
    height: 200%;
    background-color: #399953;
    background-repeat: no-repeat;
    background-size: 50% 50%, 50% 50%;
    background-position: 0 0, 100% 0, 100% 100%, 0 100%;
    background-image: linear-gradient(#646cff, #646cff), linear-gradient(#1f2937, #1f2937), linear-gradient(#646cff, #646cff), linear-gradient(#1f2937, #1f2937);
    animation: rotate 7s linear infinite;
  }
  
  &::after {
    content: '';
    position: absolute;
    z-index: -1;
    left: 6px;
    top: 6px;
    width: calc(100% - 12px);
    height: calc(100% - 12px);
    background: #1f2937;
    border-radius: 5px;
  }
}

@keyframes opacityChange {
  50% {
    opacity: 1;
  }
  100% {
    opacity: .5;
  }
}
h1 {
    font-size: 2.2em!important;
}
</style>