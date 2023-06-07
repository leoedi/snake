<template>
  <div class="q-pa-md q-gutter-sm">
    <q-dialog v-model="alert">
      <q-card>
        <q-card-section>
          <div class="text-h6">Alert</div>
        </q-card-section>

        <q-card-section class="q-pt-none"> {{ information }} </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    return {
      alert: ref(false),
      information: ref(""),
    };
  },
};
</script>
