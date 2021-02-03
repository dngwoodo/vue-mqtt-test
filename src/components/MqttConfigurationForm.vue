<template>
  <div>
    <div class="emq-title">
      configuration
    </div>
    <el-form
      ref="configForm"
      hide-required-asterisk
      size="small"
      label-position="top"
      :model="connection"
    >
      <el-row :gutter="20">
        <el-col :span="8">
          <el-form-item prop="host" label="Host">
            <el-input v-model="connection.host"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item prop="port" label="Port">
            <el-input
              v-model.number="connection.port"
              type="number"
              placeholder="8083/8084"
            ></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item prop="endpoint" label="Mountpoint">
            <el-input
              v-model="connection.endpoint"
              placeholder="/mqtt"
            ></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item prop="clientId" label="Client ID">
            <el-input v-model="connection.clientId"> </el-input>
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item prop="username" label="Username">
            <el-input v-model="connection.username"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item prop="password" label="Password">
            <el-input v-model="connection.password"></el-input>
          </el-form-item>
        </el-col>

        <el-col :span="24">
          <el-button
            type="success"
            size="small"
            class="conn-btn"
            style="margin-right: 20px;"
            :disabled="client.connected"
            @click="createConnection"
          >
            {{ client.connected ? "Connected" : "Connect" }}
          </el-button>

          <el-button
            v-if="client.connected"
            type="danger"
            size="small"
            class="conn-btn"
            @click="destroyConnection"
          >
            Disconnect
          </el-button>
        </el-col>
      </el-row>
    </el-form>
  </div>
</template>

<script>
import mqtt from "mqtt";

export default {
  props: {
    client: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      connection: {
        host: "broker.emqx.io",
        port: 8083,
        endpoint: "/mqtt",
        clean: true, // 세션유지
        connectTimeout: 4000, // 연결시간 초과
        reconnectPeriod: 4000, // 재연결 간격
        // 인증정보
        clientId: "mqttjs_3be2c321",
        username: "emqx_test",
        password: "emqx_test"
      }
    };
  },
  methods: {
    createConnection() {
      const { host, port, endpoint, ...options } = this.connection;
      const connectUrl = `ws://${host}:${port}${endpoint}`; // 브로커에 연결 url
      let client = {};
      try {
        client = mqtt.connect(connectUrl, options); // 브로커 연결 및 Mqtt 인스턴스 생성
      } catch (error) {
        console.log(`커넥트 실패`, error); // 연결 실패
      }
      client.on("connect", () => {
        console.log("커넥션 성공!"); // connect 이벤트 발생 시
        console.log(client.connected);
      });
      client.on("error", error => {
        console.log("커넥션 에러", error); // connect 에러 발생 시
      });
      client.on("message", (topic, message) => {
        this.receiveNews = this.receiveNews.concat(message); // 메시지가 올 시
        console.log(`Received message ${message} from topic ${topic}`);
      });
      this.$emit("update:connection", client);
    },
    destroyConnection() {
      if (this.client.connected) {
        try {
          this.client.end(); // 연결 끊기
          this.$emit("update:connection", { connected: false }); // connected 초기화
        } catch (error) {
          console.log("연결끊기 실패", error.toString());
        }
      }
    }
  }
};
</script>

<style lang="scss" scoped></style>
