<template>
  <el-form
    ref="publish"
    hide-required-asterisk
    size="small"
    label-position="top"
    :model="publish"
  >
    <el-row :gutter="20">
      <el-col :span="8">
        <el-form-item prop="topic" label="Topic">
          <el-input v-model="publish.topic"></el-input>
        </el-form-item>
      </el-col>
      <el-col :span="8">
        <el-form-item prop="payload" label="Payload">
          <el-input v-model="publish.payload" size="small"></el-input>
        </el-form-item>
      </el-col>
      <el-col :span="8">
        <el-form-item prop="qos" label="QoS">
          <el-select v-model="publish.qos">
            <el-option
              v-for="(item, index) in qosList"
              :key="index"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col style="display: flex; justify-content: flex-end;">
        <el-button
          :disabled="!client.connected"
          type="success"
          size="small"
          class="publish-btn"
          @click="doPublish"
        >
          Publish
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
      publish: {
        topic: "topic/browser",
        qos: 0,
        payload: '{ "msg": "Hello, I am browser." }'
      },
      qosList: [
        { label: 0, value: 0 },
        { label: 1, value: 1 },
        { label: 2, value: 2 }
      ]
    };
  },
  methods: {
    doPublish() {
      const { topic, qos, payload } = this.publish;
      this.client.publish(topic, payload, qos, error => {
        if (error) {
          console.log("Publish error", error);
        }
        console.log("Publish 성공");
      });
    }
  }
};
</script>
