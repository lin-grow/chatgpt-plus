<template>
  <el-container class="captcha-box">
    <el-button type="primary" class="send-btn" :size="props.size" :disabled="!canSend" @click="loadCaptcha" plain>
      {{ btnText }}
    </el-button>
  </el-container>
</template>

<script setup>
// 发送短信验证码组件
import {ref} from "vue";
import {validateEmail, validateMobile} from "@/utils/validate";
import {ElMessage} from "element-plus";
import {httpPost} from "@/utils/http";

const props = defineProps({
  receiver: String,
  size: String,
});
const btnText = ref('发送验证码')
const canSend = ref(true)
const captKey = ref('')
const dots = ref(null)



const loadCaptcha = () => {
  if (!validateMobile(props.receiver) && !validateEmail(props.receiver)) {
    return ElMessage.error("请输入合法的手机号/邮箱地址")
  }
  sendMsg()
}

const sendMsg = () => {
  if (!canSend.value) {
    return
  }

  canSend.value = false
  httpPost('/api/sms/code', {receiver: props.receiver}).then(() => {
    ElMessage.success('验证码发送成功')
    let time = 120
    btnText.value = time
    const handler = setInterval(() => {
      time = time - 1
      if (time <= 0) {
        clearInterval(handler)
        btnText.value = '重新发送'
        canSend.value = true
      } else {
        btnText.value = time
      }
    }, 1000)
  }).catch(e => {
    canSend.value = true
    ElMessage.error('验证码发送失败：' + e.message)
  })
}

</script>

<style lang="stylus">

.captcha-box {
  .send-btn {
    width: 100%;
  }

  .el-dialog {
    .el-dialog__header {
      padding: 0;
    }

    .el-dialog__body {
      //padding 0
    }
  }
}
</style>