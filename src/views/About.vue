<template>
  <div>
    <v-container>
      <qrcode-vue :value="value"> </qrcode-vue>
      <v-text-field
        class="mt-10"
        v-model="value"
        label="Outlined"
        outlined
        clearable
      ></v-text-field>
    </v-container>
  </div>
</template>
<script>
import QrcodeVue from "qrcode.vue";

export default {
  data() {
    return {
      value: "www.example.com",
      size: 300,
    };
  },
  components: {
    "qrcode-vue": QrcodeVue,
  },
};
</script>
