<template>
  <div class="mb-3">
    <h2>Create Employees</h2>
  </div>
  <v-card flat class="border pb-4">
    <Form-Create
      :payload="payload"
      :loading="isLoadingSubmit"
      @submit="postEmployees"
    ></Form-Create>
  </v-card>
</template>

<script setup>
import moment from "moment";
import { reactive, ref, useTemplateRef } from "vue";
import { useRouter } from "vue-router";
import FormCreate from "@/components/form/index.vue";

const router = useRouter();

const url = "http://localhost:3001/employees";

const today = moment().format("DDMMYYYY");
const nipNumber = ref("001");

const payload = ref({
  nip: "",
  firstName: "",
  lastName: "",
  address: "",
  position: "",
  salary: "",
  division: "",
  birthDate: "",
  joinDate: "",
});

const isLoadingSubmit = ref(false);

async function postEmployees() {
  const formattedPayload = {
    ...payload.value,
    birthDate: moment(payload.birthDate).format("DD-MM-YYYY"),
    joinDate: moment(payload.joinDate).format("DD-MM-YYYY"),
  };

  isLoadingSubmit.value = true;
  try {
    const response = await fetch(url, {
      method: "POST",
      body: JSON.stringify(formattedPayload),
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

async function getEmployees() {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }
    const json = await response.json();
    for (let i = 0; i < json.length; i++) {
      let nipArray = json[i].nip.split("-");
      if (
        nipArray[1] == today &&
        parseInt(nipNumber.value) < parseInt(nipArray[2]) + 1
      ) {
        nipNumber.value =
          parseInt(nipArray[2]) + 1 > 9
            ? `0${parseInt(nipArray[2]) + 1}`
            : parseInt(nipArray[2]) + 1 > 99
            ? `${parseInt(nipArray[2]) + 1}`
            : `00${parseInt(nipArray[2]) + 1}`;
      }
    }
    payload.value.nip = `AQI-${today}-${nipNumber.value}`;
  } catch (error) {
    console.error(error.message);
    alert(error.message)
  }
}

onMounted(getEmployees);
</script>
