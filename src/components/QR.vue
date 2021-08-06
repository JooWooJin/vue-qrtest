<template>
  <div>
    <p class="error">{{ error }}</p>
    <p class="decode-result">Last result: <b>{{ result }}</b></p> 
   <qrcode-stream @decode="onDecode" @init="onInit" />
  </div>
</template>

<script type="module">
</script>

<script>

export default {

  components:{
    VueQrcodeReader
  },

  data () {
    return {
      result: '',
      error: ''
    }
  },
  watch:{

  },
  methods: {
    onDecode (result) {
      this.result = result
    },

    async onInit (promise) {
      try {
        await promise
      } catch (error) {
        if (error.name === 'NotAllowedError') {
          this.error = "ERROR: 카메라 권한을 허용해주세요"
        } else if (error.name === 'NotFoundError') {
          this.error = "ERROR: 접속중인 기기에 카메라를 찾을 수 없습니다."
        } else if (error.name === 'NotSupportedError') {
          this.error = "ERROR: 보안환경에서만 작동합니다."
        } else if (error.name === 'NotReadableError') {
          this.error = "ERROR: 카메라가 준비된 상태가 아닙니다."
        } else if (error.name === 'OverconstrainedError') {
          this.error = "ERROR: 설치된 카메라가 적합하지 않습니다."
        } else if (error.name === 'StreamApiNotSupportedError') {
          this.error = "ERROR: 해당 브라우저에서 지원되지 않는 기능입니다."
        }
      }
    }
  }
}
</script>

<style scoped>
.error {
  font-weight: bold;
  color: red;
}
</style>
