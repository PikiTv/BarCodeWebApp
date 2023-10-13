<template>
  <div>
    <StreamBarcodeReader @decode="onDecode" @loaded="onLoaded" class="camera-container"></StreamBarcodeReader>
    <p v-if="scannedBarcode">Gescannter Barcode: {{ scannedBarcode }}</p>
  </div>
</template>

<script>
import { StreamBarcodeReader } from "vue-barcode-reader";

export default {
  components: {
    StreamBarcodeReader
  },
  data() {
    return {
      scannedBarcode: null
    };
  },
  methods: {
    onDecode(decodedData) {
      // Diese Methode wird aufgerufen, wenn ein Barcode gescannt wurde.
      this.scannedBarcode = decodedData;
    },
    onLoaded() {
      // Diese Methode wird aufgerufen, wenn die Kamera geladen ist.
      console.log("Kamera ist geladen.");
    }
  }
};
</script>

<!--<style>
.camera-container {
  transform: scaleX(-1); /* Spiegele die Kameraanzeige horizontal */
}
</style>
-->

<style>
@media (min-width: 1500px) {
  /* Füge die Spiegelung nur auf Bildschirmen mit einer Mindestbreite von 768 Pixeln hinzu (Computer) */
  .camera-container {
    transform: scaleX(-1); /* Spiegele die Kameraanzeige horizontal */
  }
}
</style>