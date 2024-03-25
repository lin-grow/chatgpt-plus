<template>
  <div class="reset-pass">
    <el-dialog
        v-model="showDialog"
        :close-on-click-modal="true"
        width="240px"
        :before-close="close"
        :title="title"
    >
      <div class="wechat-card">
        <el-image :src="qrCodeUrl"/>
      </div>
    </el-dialog>
  </div>
</template>

<script setup>
import {computed, ref} from "vue";

const props = defineProps({
  show: Boolean,
  mobile: String
});

const showDialog = computed(() => {
  return props.show
})

const title = ref('联系客服重置密码')
const qrCodeUrl = ref("/images/wx.png")

const emits = defineEmits(['hide']);

const close = function () {
  emits('hide', false);
}
</script>

<style lang="stylus">

</style>