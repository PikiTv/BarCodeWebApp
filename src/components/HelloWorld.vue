<template>
  <h1>Barcode Scanner</h1>
  <div>
    <StreamBarcodeReader @decode="onDecode" @loaded="onLoaded" class="camera-container"></StreamBarcodeReader>
    <p v-if="scannedBarcode">Gescannter Barcode: {{ scannedBarcode }}</p>
    <img v-if="scannedBarcode && images[scannedBarcode]" :src="images[scannedBarcode]" alt="Scanned Product Image" />
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
      scannedBarcode: null,
      product: ["5060896625829", "90357350", "5000159472029"],
      images: {
        "5060896625829": "https://images.gopuff.com/blob/gopuffcatalogstorageprod/catalog-images-container/resize/cf/version=1_0,format=auto,fit=scale-down,width=800,height=800/dbbe25d8-813d-4743-a256-1e7d4f1b0fae.png",
        "90357350": "https://img.rewe-static.de/6652816/4778180_digital-image.png",
        "5000159472029": "https://www.scheiding-online.de/product/image/medium/613014_m%26m-crispy-213g.png",
      }
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

<style>
@media (min-width: 1500px) {
  /* Füge die Spiegelung nur auf Bildschirmen mit einer Mindestbreite von 768 Pixeln hinzu (Computer) */
  .camera-container {
    transform: scaleX(-1); /* Spiegele die Kameraanzeige horizontal */
  }
}
img{
  max-width: 300px;
  max-height: 300px;
}
</style>