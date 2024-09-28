<template>
  <div class="mb-3">
    <h2>Employees Management</h2>
  </div>
  <v-card flat class="border">
    <section class="d-flex flex-column flex-sm-row align-sm-center pa-3 ga-3">
      <v-text-field
        v-model="filter"
        label="Search"
        prepend-inner-icon="mdi-magnify"
        variant="outlined"
        density="compact"
        hide-details
        single-line
      ></v-text-field>
      <v-btn
        to="/create"
        flat
        class="text-capitalize text-subtitle-2"
        variant="tonal"
        color="primary"
        prepend-icon="mdi-plus"
        >Create</v-btn
      >
    </section>
    <v-data-table
      :headers="headers"
      :items="dataEmployees"
      :search="filter"
      disable-sort
      hide-default-footer
      :items-per-page="0"
      :loading="isLoadingDataEmployee"
    >
      <template v-slot:item.salary="{ item }">
        <span>{{ formattingCurrency(item.salary) }}</span>
      </template>
      <template v-slot:item.action="{ item }">
        <div class="d-flex align-center ga-2">
          <v-btn
            flat
            class="text-capitalize text-subtitle-2"
            variant="outlined"
            :to="item.id"
          >
            Detail
          </v-btn>
          <v-btn
            flat
            class="text-capitalize text-subtitle-2"
            variant="tonal"
            color="primary"
            prepend-icon="mdi-pencil"
            :to="`edit/${item.id}`"
            >Edit</v-btn
          >
          <v-btn
            flat
            class="text-capitalize text-subtitle-2"
            variant="tonal"
            color="error"
            prepend-icon="mdi-close"
            @click="deleteEmployee(item.id)"
            :loading="isLoadingDelete"
            >Delete</v-btn
          >
        </div>
      </template>
    </v-data-table>
  </v-card>
</template>

<script setup>
import { onMounted, ref } from "vue";

const url = "http://localhost:3001/employees";

const headers = ref([
  { key: "nip", title: "NIP", minWidth: 200, maxWidth: 200 },
  { key: "firstName", title: "First Name", minWidth: 120, maxWidth: 120 },
  { key: "lastName", title: "Last Name", minWidth: 120, maxWidth: 120 },
  { key: "address", title: "Address", minWidth: 200, maxWidth: 200 },
  { key: "position", title: "Position", minWidth: 200, maxWidth: 200 },
  { key: "salary", title: "Salary", minWidth: 180, maxWidth: 180 },
  { key: "division", title: "Division", minWidth: 140, maxWidth: 140 },
  { key: "birthDate", title: "Birth Date", minWidth: 120, maxWidth: 120 },
  { key: "joinDate", title: "Join Date", minWidth: 120, maxWidth: 120 },
  { key: "action", title: "", minWidth: 220, maxWidth: 300 },
]);

const dataEmployees = ref([]);
const filter = ref("");
const isLoadingDataEmployee = ref(false);
const isLoadingDelete = ref(false);

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
    getEmployee();
  } catch (error) {
    console.error(error.message);
  }
}

async function getEmployee() {
  dataEmployees.value = [];
  isLoadingDataEmployee.value = true;
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const json = await response.json();
    dataEmployees.value = json;
    isLoadingDataEmployee.value = false;
  } catch (error) {
    alert(error.message)
    isLoadingDataEmployee.value = false;
  }
}

onMounted(getEmployee);
</script>
