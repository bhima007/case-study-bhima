<template>
  <div>
    <section class="mb-3">
      <h2>Detail Employees</h2>
    </section>
    <v-card flat class="border">
      <section class="px-4 pb-4 overflow-auto">
        <div v-for="(data, key) in dataEmployees" :key="data.id" class="d-flex">
          <div v-if="key != 'id'" style="min-width: 140px" class="py-2">
            <div class="text-capitalize">
              {{ formattingKeyByCamelCase(key) }}
            </div>
          </div>
          <div v-if="key != 'id'" style="min-width: 300px" class="py-2">
            <span>:&nbsp;</span>
            <span v-if="key == 'salary'">
              {{ formattingCurrency(data) }}
            </span>
            <span v-else>
              {{ data }}
            </span>
          </div>
        </div>
      </section>
    </v-card>

    <section class="d-flex ga-3 mt-3">
      <v-btn
        flat
        class="text-capitalize text-subtitle-2"
        variant="tonal"
        color="primary"
        prepend-icon="mdi-pencil"
        :to="`edit/${route.params.id}`"
        >Edit</v-btn
      >
      <v-btn
        flat
        class="text-capitalize text-subtitle-2"
        variant="tonal"
        color="error"
        prepend-icon="mdi-close"
        @click="deleteEmployee(route.params.id)"
        :loading="isLoadingDelete"
        >Delete</v-btn
      >
    </section>
  </div>
</template>

<script setup>
import moment from "moment";
import { onMounted, ref } from "vue";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();

const url = "http://localhost:3001/employees";

const dataEmployees = ref({});
const isLoadingDelete = ref(false);

const formattingKeyByCamelCase = (v) => {
  return v.replace(/([a-z0-9])([A-Z])/g, "$1 $2");
};

const formattingCurrency = (number) => {
  return new Intl.NumberFormat("id-ID", {
    style: "currency",
    currency: "IDR",
  }).format(number);
};

async function deleteEmployee(id) {
  isLoadingDelete.value = true;
  try {
    const response = await fetch(`${url}/${id}`, {
      method: "DELETE",
    });
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    // const json = await response.json();
    // dataEmployees.value = json;

    isLoadingDelete.value = false;
    router.push("/");
  } catch (error) {
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
    dataEmployees.value = json;
  } catch (error) {
    console.error(error.message);
    alert(error.message)
  }
}

onMounted(getEmployeesById);
</script>
