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
import axios from "axios"; // Fügen Sie axios hinzu

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
  created() {
    // Beim Erstellen der Komponente die gescannten Daten vom Backend abrufen
    this.fetchScannedBarcode();
  },
  methods: {
    onDecode(decodedData) {
      this.scannedBarcode = decodedData;
      // Speichern Sie den gescannten Barcode im Backend
      this.saveScannedBarcode(decodedData);
    },
    onLoaded() {
      console.log("Kamera ist geladen.");
    },
    async fetchScannedBarcode() {
      try {
        const response = await axios.get("/api/scanned-barcode");
        if (response.data && response.data.scannedBarcode) {
          this.scannedBarcode = response.data.scannedBarcode;
        }
      } catch (error) {
        console.error("Fehler beim Abrufen des gescannten Barcodes:", error);
      }
    },
    async saveScannedBarcode(barcode) {
      try {
        await axios.post("/api/scanned-barcode", { scannedBarcode: barcode });
        console.log("Gespeicherter Barcode:", barcode); // Ausgabe in der Konsole
      } catch (error) {
        console.error("Fehler beim Speichern des gescannten Barcodes:", error);
      }
    },
  }
};
</script>

<style>
@media (min-width: 1500px) {
  .camera-container {
    transform: scaleX(-1); 
  }
}
img {
  max-width: 300px;
  max-height: 300px;
}
</style>