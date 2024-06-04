<template>
  <div class="flex justify-center items-center h-screen">
    <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
      <div class="mb-4">
        <label class="block text-gray-700 text-sm font-bold mb-2" for="rut">
          Rut holding
        </label>
        <input
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          id="rut"
          type="text"
          placeholder="Rut holding"
          v-model="rut"
        />
      </div>
      <div class="mb-4">
        <label
          class="block text-gray-700 text-sm font-bold mb-2"
          for="username"
        >
          Username
        </label>
        <input
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          id="username"
          type="text"
          placeholder="Username"
          v-model="username"
        />
      </div>
      <div class="mb-6">
        <label
          class="block text-gray-700 text-sm font-bold mb-2"
          for="password"
        >
          Password
        </label>
        <input
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
          id="password"
          type="password"
          placeholder="******************"
          v-model="password"
        />
      </div>
      <div class="flex items-center justify-between">
        <button
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="button"
          @click="loginWithAxios()"
        >
          Ingresar
        </button>
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
import router from "@/router";
import { ref } from "vue";
import axios from "axios";

const rut = ref("76494210-8");
const username = ref("victorortiz74@gmail.com");
const password = ref("123456");

async function login() {
  if (!username.value || !password.value || !rut.value) {
    alert("Error: Debes llenar todos los campos");
    return;
  }

  const response = await fetch(
    "http://ofis.helpcom.cl:12086/ms-admin/auth/login",
    {
      method: "POST",
      credentials: "include",
      headers: {
        email: username.value,
        password: password.value,
        rutHolding: rut.value,
      },
      //body: JSON.stringify({ username: username.value, password: password.value }),
    }
  );
  if (response.ok) {
    console.log("Autenticación exitosa");
    const data = await response.json();
    router.push({
      name: "User",
      params: { id: data.id },
    });
  } else {
    alert("Error: Credenciales incorrectas");
  }
}

//Login con axios
async function loginWithAxios() {
  if (!username.value || !password.value || !rut.value) {
    alert("Error: Debes llenar todos los campos");
    return;
  }

  try {
    const response = await axios({
      method: 'POST',
      url: 'http://ofis.helpcom.cl:12086/ms-admin/auth/login',
      withCredentials: true,
      headers: {
        email: username.value,
        password: password.value,
        rutHolding: rut.value
      }
    });

    if (response.status === 200) {
      console.log("Autenticación exitosa");
      const data = await response.data;
      router.push({
        name: "User",
        params: { id: data.id }
      });
    } else {
      alert("Error: Credenciales incorrectas");
    }
  } catch (error) {
    alert("Error al realizar la petición");
  }
}
</script>