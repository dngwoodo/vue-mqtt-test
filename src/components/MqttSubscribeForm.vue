<template>
  <el-form
    ref="subscription"
    hide-required-asterisk
    size="small"
    label-position="top"
    :model="subscription"
  >
    <el-row :gutter="20">
      <el-col :span="8">
        <el-form-item prop="topic" label="Topic">
          <el-input v-model="subscription.topic"></el-input>
        </el-form-item>
      </el-col>
      <el-col :span="8">
        <el-form-item prop="qos" label="QoS">
          <el-select v-model="subscription.qos">
            <el-option
              v-for="(item, index) in qosList"
              :key="index"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="8">
        <el-button
          :disabled="!client.connected"
          type="success"
          size="small"
          class="subscribe-btn"
          @click="doSubscribe"
        >
          {{ subscribeSuccess ? "Subscribed" : "Subscribe" }}
        </el-button>
        <el-button
          :disabled="!client.connected"
          type="danger"
          size="small"
          class="subscribe-btn"
          style="margin-left:20px"
          @click="doUnSubscribe"
          v-if="subscribeSuccess"
        >
          Unsubscribe
        </el-button>
      </el-col>
    </el-row>
  </el-form>
</template>

<script>
export default {
  props: {
    client: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      subscription: {
        topic: "topic/mqttx",
        qos: 0
      },
      qosList: [
        { label: 0, value: 0 },
        { label: 1, value: 1 },
        { label: 2, value: 2 }
      ],
      subscribeSuccess: false
    };
  },
  methods: {
    doSubscribe() {
      const { topic, qos } = this.subscription;
      this.client.subscribe(topic, { qos }, (error, res) => {
        if (error) {
          console.log("토픽 구독 실패", error);
          return;
        }
        this.subscribeSuccess = true;
        console.log("토픽 구독 성공", res);
      });
    },
    doUnSubscribe() {
      const { topic } = this.subscription;
      this.client.unsubscribe(topic, error => {
        if (error) {
          console.log("구독 취소 에러", error);
        }
        this.subscribeSuccess = false;
        console.log("구독 취소 성공");
      });
    }
  }
};
</script>
