<template>
  <q-page class="flex flex-center fondo" style="padding: 1em">
    <q-card
      style="
        text-align: center;
        background: rgba(255, 255, 255, 0.4);
        max-width: 600px;
      "
    >
      <q-card-section>
        <img
          alt="Quasar logo"
          src="https://app.tarjetajovendiamante.com/assets/logo.683026de.svg"
          style="width: 200px; height: 200px"
        />
        <!--Formulario-->
        <q-form @submit="onSubmit" @reset="onReset" class="row">
          <div class="col-12 col-md-6" style="padding: 0.5em">
            <q-input
              rounded
              filled
              v-model="formulario.name"
              label="Tu nombre *"
              hint="Tu nombre"
              lazy-rules
              :rules="[
                (val) =>
                  (val && val.length > 0) || 'Por favor ingresa tu nombre',
              ]"
            />
          </div>
          <div class="col-12 col-md-6" style="padding: 0.5em">
            <q-input
              rounded
              filled
              v-model="formulario.last_name"
              label="Tu apellido *"
              hint="Tu apellido"
              lazy-rules
              :rules="[
                (val) =>
                  (val && val.length > 0) || 'Por favor ingresa tu apellido',
              ]"
            />
          </div>
          <div class="col-12 col-md-6" style="padding: 0.5em">
            <q-input
              rounded
              filled
              v-model="formulario.dni"
              label="Tu cedula *"
              hint="Tu cedula"
            />
          </div>
          <div class="col-12 col-md-6" style="padding: 0.5em">
            <q-input
              rounded
              filled
              type="email"
              v-model="formulario.email"
              label="Tu email *"
              hint="Tu email"
              lazy-rules
              :rules="[
                (val) =>
                  (val && val.length > 0) || 'Por favor ingresa tu email',
              ]"
            />
          </div>
          <div class="col-12 col-md-6" style="padding: 0.5em">
            <q-select
              rounded
              filled
              v-model="formulario.type_contact"
              :options="types"
              label="Tipos de contacto"
            />
          </div>
          <div class="col-12 col-md-6" style="padding: 0.5em">
            <q-select
              rounded
              filled
              v-model="formulario.vendedor"
              :options="vendedor"
              label="Persona que lo atendio"
            />
          </div>
          <div class="col-12 col-md-6" style="padding: 0.5em">
            <q-select
              rounded
              filled
              v-model="formulario.place_you_frequent"
              :options="lugares"
              label="Lugares que frecuentas"
            />
          </div>
          <div class="col-12">
            <h1 class="text-h6">Rango de edad</h1>
            <q-radio
              keep-color
              v-model="formulario.age_range"
              val="18 o menos"
              label="18 o menos"
              color="teal"
            />
            <q-radio
              keep-color
              v-model="formulario.age_range"
              val="18 a 25"
              label="18 a 25"
              color="orange"
            />
            <q-radio
              keep-color
              v-model="formulario.age_range"
              val="26 a 35"
              label="26 a 35"
              color="red"
            />
            <q-radio
              keep-color
              v-model="formulario.age_range"
              val="36 o más"
              label="36 o más"
              color="cyan"
            />
          </div>
          <div class="col-12" style="padding: 1em">
            <q-btn label="Submit" type="submit" color="primary" />
            <q-btn label="Reset" type="reset" color="red" class="q-ml-sm" />
          </div>
        </q-form>
        <!--END-->
      </q-card-section>
    </q-card>
  </q-page>
</template>
<style>
.fondo {
  background-image: url("https://form.tarjetajovendiamante.com/fondo.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}
</style>
<script setup>
import { data } from "browserslist";
import { useQuasar } from "quasar";
import { ref } from "vue";
import { api } from "../boot/axios.js";
import { Notify } from "quasar";

const $q = useQuasar();
const formulario = ref({
  vendedor: "",
  name: "",
  last_name: "",
  dni: " ",
  email: "",
  type_contact: "",
  age_range: "",
  place_you_frequent: "",
});
const types = ["Ventas", "Pendiente", "Prospecto", "Referido o voluntario"];
const vendedor = [];
for (let i = 1; i < 21; i++) {
  vendedor.push("Vendedor " + i);
}
const lugares = [
  "Panama Este",
  "Panama Oeste",
  "Panama Centro",
  "Chiriqui",
  "Otros",
];
const accept = ref(false);

const onSubmit = async () => {
  if (
    formulario.value.name == "" ||
    formulario.value.last_name == "" ||
    formulario.value.email == "" ||
    formulario.value.type_contact == "" ||
    formulario.value.age_range == "" ||
    formulario.value.place_you_frequent == "" ||
    formulario.value.vendedor == ""
  ) {
    $q.notify({
      color: "red-4",
      textColor: "white",
      icon: "warning",
      message: "Por favor llene todos los campos",
    });
  } else {
    if (
      formulario.value.dni === "" ||
      formulario.value.dni === " " ||
      formulario.value.dni === null
    ) {
      formulario.value.dni = "N/A";
    }
    await api.post("formulario2", formulario.value).then((response) => {
      if (response.data.id) {
        $q.notify({
          color: "green-4",
          textColor: "white",
          icon: "done",
          message: "Formulario enviado con exito",
        });
        onReset();
      } else {
        $q.notify({
          color: "red-4",
          textColor: "white",
          icon: "warning",
          message: "Error al enviar el formulario",
        });
      }
    });
  }
};

const onReset = () => {
  formulario.value = {
    vendedor: "",
    name: "",
    last_name: "",
    dni: " ",
    email: "",
    type_contact: "",
    age_range: "",
    place_you_frequent: "",
  };
  accept.value = false;
};
</script>
