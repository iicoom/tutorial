<script setup lang="ts">
import { useRegisterSW } from 'virtual:pwa-register/vue'
import upgrade from '@/assets/upgrade.png'
import GlobalFunction from '@/utils/GlobalFunction'

const { offlineReady, needRefresh, updateServiceWorker } = useRegisterSW()

const close = async () => {
  offlineReady.value = false
  needRefresh.value = false
}
//@ts-ignore
const version = __APP_VERSION
const platform = GlobalFunction.getPlatform()
</script>

<template>
  <el-dialog
    class="update-tip"
    v-model="needRefresh"
    title=""
    :width="platform === 'pc' ? '500px' : '80%'"
    draggable
    overflow
    :show-close="false"
  >
    <img :src="upgrade" />
    <h3>发现新版本！</h3>  
    <p>如遇更新失败请手动刷新 Windows(Ctrl+F5)<br> Mac(Command+Shift+R) 重试</p>
    <template #footer>
      <div class="dialog-footer">
        <el-button round @click="close">取消</el-button>
        <el-button v-if="needRefresh" type="primary" round @click="updateServiceWorker()">更新</el-button>
      </div>
    </template>
  </el-dialog>
</template>

<style lang="scss">
/* UI更新 */
.update-tip {
  padding-top: 50px;
  border-radius: 20px!important;
  user-select: none;
  img {
    width: 130px;
    position: absolute;
    left: 50%;
    top: -45px;
    transform: translateX(-50%);
  }
  h3 {
    font-size: 20px;
    margin-bottom: 20px;
    margin-top: 65px;
    text-align: center;
  }
  p {
    font-size: 14px;
    text-align: center;
  } 
}
.update-message {
  margin: 0 0 15px 0;
}
</style>
