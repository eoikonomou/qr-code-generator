<template>
  <div id="app">
      <h1>QR Code Generator</h1>
      <div class="form-group">
          <textarea
              name="qr-input"
              id="qr-input"
              cols="30"
              rows="10"
              placeholder="Enter QR data..."
              v-model="qrData"
              @input="updateQR"
          />
      </div>
      <div v-show="qrSvg" class="qr-preview">
          <div>Preview</div>
          <div id="qr-preview"></div>
          <button @click="exportQR">Export</button>
          <a id="anchor"></a>
      </div>
  </div>
</template>

<script>
import QRCode from 'qrcode-svg';
export default {
  name: 'App',
  components: {},
    data() {
      return {
          qrData: '',
          qrSvg: null
      }
    },
    computed: {
      qrElement() {
          return document.getElementById('qr-preview')
      }
    },
    methods: {
    updateQR(e) {
        if (e.target.value) {
            this.qrSvg = new QRCode({
                content: this.qrData,
                join: true
            }).svg()
            this.qrElement.innerHTML = this.qrSvg
        } else {
            this.qrSvg = null
        }
    },
      async exportQR() {
          try {
              const blob = new Blob(
                  [this.qrSvg],
                  { type: 'image/svg' }
              )
              const link = document.getElementById('anchor')
              link.href = URL.createObjectURL(blob)
              link.download = 'qr-code.svg'
              link.click()
          } catch (e) {
              console.log('Error exporting QR code:', e)
          }
      }
    }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

</style>
