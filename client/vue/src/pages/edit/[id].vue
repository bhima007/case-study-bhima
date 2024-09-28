<template>
  <div>
    <div class="mb-3">
      <h2>Edit Employees</h2>
    </div>
    <v-card flat class="border pb-4">
      <Form-Edit
        :payload
        :loading="isLoadingSubmit"
        @submit="updateEmployeesById"
      ></Form-Edit>
    </v-card>
  </div>
</template>

<script setup>
import FormEdit from "@/components/form/index.vue";
import moment from "moment";
import { ref } from "vue";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();

const url = "http://localhost:3001/employees";

const payload = ref({});
const isLoadingSubmit = ref(false);

async function updateEmployeesById() {
  isLoadingSubmit.value = true;
  try {
    const response = await fetch(`${url}/${route.params.id}`, {
      method: "PUT",
      body: JSON.stringify({
        ...payload.value,
        birthDate: moment(payload.value.birthDate).format("DD-MM-YYYY"),
        joinDate: moment(payload.value.joinDate).format("DD-MM-YYYY"),
      }),
    });
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const json = await response.json();
    isLoadingSubmit.value = false;
    router.back();
  } catch (error) {
    isLoadingSubmit.value = false;
    console.error(error.message);
    alert(error.message)
  }
}

async function getEmployeesById() {
  try {
    const response = await fetch(`${url}/${route.params.id}`);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }
    const json = await response.json();
    payload.value = {
      ...json,
      birthDate: moment(json.birthDate, "DD-MM-YYYY").format("YYYY-MM-DD"),
      joinDate: moment(json.joinDate, "DD-MM-YYYY").format("YYYY-MM-DD"),
    };
    console.log({ json }, payload.value);
  } catch (error) {
    console.error(error.message);
    alert(error.message)
  }
}

onMounted(getEmployeesById);
</script>
