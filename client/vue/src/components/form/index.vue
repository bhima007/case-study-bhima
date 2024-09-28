<template>
  <v-form ref="form"
    >
    <section class="d-flex flex-column ga-3 mt-3 px-4">
      <v-text-field
        v-model="payload.nip"
        variant="outlined"
        density="compact"
        label="NIP"
        hide-details
        :disabled="route.path.includes('edit')"
        max-width="400"
      ></v-text-field>

      <div class="d-flex flex-column flex-sm-row ga-3" style="max-width: 400px">
        <v-text-field
          v-model="payload.firstName"
          variant="outlined"
          density="compact"
          label="First name"
          hide-details
          :rules="rules"
          required
        ></v-text-field>

        <v-text-field
          v-model="payload.lastName"
          variant="outlined"
          density="compact"
          label="Last name"
          hide-details
          :rules="rules"
          required
        ></v-text-field>
      </div>

      <v-text-field
        v-model="payload.address"
        variant="outlined"
        density="compact"
        label="Address"
        hide-details
        :rules="rules"
        required
        max-width="400"
      ></v-text-field>

      <v-text-field
        v-model="payload.position"
        variant="outlined"
        density="compact"
        label="Position"
        hide-details
        :rules="rules"
        required
        max-width="400"
      ></v-text-field>

      <v-text-field
        v-model="payload.salary"
        variant="outlined"
        density="compact"
        label="Salary"
        hide-details
        :rules="rules"
        required
        max-width="400"
        type="number"
      ></v-text-field>

      <v-text-field
        v-model="payload.division"
        variant="outlined"
        density="compact"
        label="Division"
        hide-details
        :rules="rules"
        required
        max-width="400"
      ></v-text-field>

      <div class="d-flex flex-column flex-sm-row ga-3" style="max-width: 400px">
        <v-text-field
          v-model="payload.birthDate"
          variant="outlined"
          density="compact"
          label="Birth Date"
          hide-details
          :rules="rules"
          required
          type="date"
        ></v-text-field>

        <v-text-field
          v-model="payload.joinDate"
          variant="outlined"
          density="compact"
          label="Join Date"
          hide-details
          :rules="rules"
          required
          type="date"
        ></v-text-field>
      </div>

      <div>
        <v-btn color="primary" @click="submitPayload" :loading="loading"
          >submit</v-btn
        >
      </div>
    </section>
  </v-form>
</template>

<script setup>
import { useRoute } from "vue-router";

const route = useRoute();

const props = defineProps(["payload", "loading"]);
const emit = defineEmits(["submit"]);

const rules = [(v) => !!v || "Required"];

const form = useTemplateRef("form");

const submitPayload = async () => {
  const { valid } = await form.value.validate();

  if (valid) {
    emit("submit");
  } else {
    console.log("Error");
  }
};
</script>
