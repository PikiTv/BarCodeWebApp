<template>
  <div class="container">
    <h1 class="title">Barcode Scanner</h1>
    <div class="scanner-container">
      <button @click="toggleCamera" class="scan-button">{{ isCameraOn ? 'Kamera ausschalten' : 'Hier scannen' }}</button>
      <StreamBarcodeReader ref="barcodeReader" @decode="onDecode" @loaded="onLoaded" class="camera-container" v-if="isCameraOn"></StreamBarcodeReader>
      <p v-if="scannedBarcode" class="barcode-text">Gescannter Barcode: {{ scannedBarcode }}</p>
      <img v-if="scannedBarcode && images[scannedBarcode]" :src="images[scannedBarcode]" alt="Scanned Product Image" class="product-image" />
      <div v-if="savedDataArray.length > 0" class="saved-barcodes-container">
        <h2 class="saved-barcodes-title">Gespeicherte Barcodes aus dem Backend:</h2>
        <div class="saved-barcode" v-for="(item, index) in savedDataArray" :key="index">{{ item }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import { StreamBarcodeReader } from "vue-barcode-reader";
import axios from "axios";

export default {
  components: {
    StreamBarcodeReader
  },
  data() {
    return {
      isCameraOn: false,
      scannedBarcode: null,
      images: {
        "5060896625829": "https://images.gopuff.com/blob/gopuffcatalogstorageprod/catalog-images-container/resize/cf/version=1_0,format=auto,fit=scale-down,width=800,height=800/dbbe25d8-813d-4743-a256-1e7d4f1b0fae.png",
        "90357350": "https://img.rewe-static.de/6652816/4778180_digital-image.png",
        "5000159472029": "https://www.scheiding-online.de/product/image/medium/613014_m%26m-crispy-213g.png"
      },
      savedDataArray: []
    };
  },
  created() {
    this.fetchSavedDataArray();
  },
  methods: {
    onDecode(decodedData) {
      this.scannedBarcode = decodedData;
      this.saveScannedBarcode(decodedData);
    },
    onLoaded() {
      console.log("Kamera ist geladen.");
    },
    async saveScannedBarcode(barcode) {
      try {
        await axios.post("http://localhost:8080/product", { barcode });
        console.log("Gespeicherter Barcode:", barcode);
        this.savedDataArray.push(barcode);
      } catch (error) {
        console.error("Fehler beim Speichern des gescannten Barcodes:", error);
      }
    },
    async fetchSavedDataArray() {
      try {
        const response = await axios.get("http://localhost:8080/product");
        this.savedDataArray = response.data;
      } catch (error) {
        console.error("Fehler beim Abrufen des Arrays aus dem Backend:", error);
      }
    },
    toggleCamera() {
      this.isCameraOn = !this.isCameraOn;
      if (!this.isCameraOn) {
      }
    }
  }
};
</script>

<style scoped>
@media (min-width: 1500px) {
  .camera-container {
    transform: scaleX(-1);

  }
}
.container {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  height: 100vh;
  margin-top: 20px; 
}

.title {
  font-size: 36px;
  text-align: center;
  margin-bottom: 20px;
  color: #f5f5f5;
}

.scanner-container {
  background-color: #f5f5f5;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.scan-button {
  background-color: #007bff;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-bottom: 20px;
}

.scan-button:hover {
  background-color: #0056b3;
}

.barcode-text {
  font-size: 24px;
  margin: 20px 0;
  color: #007bff;
}

.product-image {
  max-width: 300px;
  max-height: 300px;
  display: block;
  margin: 20px auto;
}

.saved-barcodes-container {
  background-color: #f5f5f5;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  margin-top: 20px;
}

.saved-barcodes-title {
  font-size: 28px;
  margin: 20px 0;
  color: #0056b3;
}

.saved-barcode {
  font-size: 20px;
  margin: 10px 0;
  color: #0056b3;
}
</style>
