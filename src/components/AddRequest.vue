<template>
  <form @submit="submit">
    <fieldset>
      <input type="text" placeholder="ID" v-model="id">
      <input type="text" placeholder="Requester" v-model="requester">
      <input type="text" placeholder="Project" v-model="project">
      <input type="text" placeholder="Cluster" v-model="cluster">
      <input type="text" placeholder="Database" v-model="database">
      <input type="text" placeholder="Request Type" v-model="requestType">
      <input type="text" placeholder="Role" v-model="role">
      <input type="text" placeholder="Status" v-model="status">
      <input type="text" placeholder="Request Date" v-model="requestedDate">
    </fieldset>
    <input class="button-primary" type="submit" value="Send">
  </form>
</template>

<script>
// import { InMemoryCache } from "apollo-cache-inmemory";
import gql from "graphql-tag";

const ADD_REQUEST = gql`
  mutation addRequest($id: ID, $requester: String, $project: String, $cluster: String, $database: String, $requestType: String, $role: String, $status: String, $requestedDate: String) {
    createUserRequestArgs(id: $id, requester: $requester, project: $project,
    cluster: $cluster, database: $database, requestType: $requestType, role: $role,
    status: $status, requestedDate: $requestedDate
    ) {
      id
      requester
      project
      cluster
      database
      requestType
      role
      status
      requestedDate
    }
    }
`;

export default {
  name: "AddRequest",
  data() {
    return {
      id: "",
      requester: "",
      project: "",
      cluster: "",
      database: "",
      requestType: "",
      role: "",
      status: "",
      requestedDate: "",
    };
  },
  apollo: {},
  methods: {
    submit(e) {
      e.preventDefault();
      const { id, requester, project, cluster, database, requestType, role, status, requestedDate } = this.$data;
      this.$apollo.mutate({
        mutation: ADD_REQUEST,
        variables: {
          id,
          requester,
          project,
          cluster,
          database,
          requestType,
          role,
          status,
          requestedDate
        },
        refetchQueries: ["getRequests"]
      });
    }
  }
};
</script>