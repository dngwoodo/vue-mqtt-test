<template>
  <div class="container">
    <el-card shadow="always" style="margin-bottom:30px;">
      <mqtt-configuration-form />
    </el-card>
    <el-card shadow="always" style="margin-bottom:30px;">
      <mqtt-subscribe-form />
    </el-card>
  </div>
</template>

<script>
import MqttConfigurationForm from "./MqttConfigurationForm.vue";
import MqttSubscribeForm from "./MqttSubscribeForm.vue";
export default {
  components: { MqttConfigurationForm, MqttSubscribeForm }
};
</script>

<style lang="scss">
@import "~@/styles";
</style>
