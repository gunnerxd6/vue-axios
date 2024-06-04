<template>
  <pre>{{ json }}</pre>
</template>

<script setup>
import { onMounted, ref } from "vue";
import { useRoute } from "vue-router";
import axios from "axios";
const route = useRoute();

const json = ref('{}');

onMounted(() => {
  getAccesos();
});
async function getAccesos() {
  try {
    const response = await axios.get(
      `http://ofis.helpcom.cl:12086/ms-admin/usuarios/accesos/${route.params.id}`,
      { withCredentials: true }
    );
    
    if (!response.status === 200) throw new Error("Error al cargar accesos");
    const data = await response.data;
    json.value = data;
  } catch (error) {
    console.error(error);
  }
}

</script>