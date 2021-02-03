<template>
  <div id="app">
    <mqtt-connection-form />
  </div>
</template>

<script>
import MqttConnectionForm from "./components/MqttConnectionForm.vue";
export default {
  name: "App",
  components: { MqttConnectionForm }
};
</script>
