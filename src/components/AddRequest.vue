<template>
  <form @submit="submit">
    <fieldset>
      <input type="text" placeholder="Requester" v-model="requester">
      <input type="text" placeholder="Project" v-model="project">
      <input type="text" placeholder="Cluster" v-model="cluster">
      <input type="text" placeholder="Database" v-model="database">
      <input type="text" placeholder="Request Type" v-model="requestType">
    </fieldset>
    <input class="button-primary" type="submit" value="Send">
  </form>
</template>

<script>
export default {
  name: "AddRequest",
  data() {
    return {
      requester: "",
      project: "",
      cluster: "",
      database: "",
      requestType: "",
      role: "",
      status: "",
      requestedDate: "",
    };
  }
};
</script>