<template>
  <div class="container">
    <el-card shadow="always" style="margin-bottom:30px;">
      <mqtt-configuration-form :client="client" :connection.sync="client" />
    </el-card>
    <el-card shadow="always" style="margin-bottom:30px;">
      <mqtt-subscribe-form :client="client" />
    </el-card>
  </div>
</template>

<script>
import MqttConfigurationForm from "./MqttConfigurationForm.vue";
import MqttSubscribeForm from "./MqttSubscribeForm.vue";
export default {
  components: { MqttConfigurationForm, MqttSubscribeForm },
  data() {
    return {
      client: {
        connected: false
      }
    };
  }
};
</script>

<style lang="scss">
@import "~@/styles";

.home-container {
  max-width: 1100px;
  margin: 0 auto;

  .conn-btn {
    color: #fff;
    background-color: #00b173;
    font-size: 14px;
  }

  .publish-btn {
    margin-bottom: 20px;
    float: right;
  }

  .el-button--success {
    background-color: #34c388 !important;
    border-color: #34c388 !important;
    font-size: 14px !important;
  }

  .el-button--danger {
    background-color: #f5222d !important;
    border-color: #f5222d !important;
  }

  .el-form-item {
    &.is-error {
      .el-input__inner,
      .el-textarea__inner {
        box-shadow: 0 0 0 2px rgba(245, 34, 45, 0.2);
      }
    }
    &.is-success {
      .el-input__inner,
      .el-textarea__inner {
        border-color: #34c388 !important;
      }
    }
  }
}
</style>
