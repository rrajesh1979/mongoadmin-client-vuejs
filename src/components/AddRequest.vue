<template>
  <form @submit="submit" >
    <div class="container-fluid">
      <div>
        <img alt="Vue logo" src="../assets/mongodb-image.png" height="200" width="320"/>
        <h5>Create new request</h5>
      </div>
      <div class="row g-3" >
          <fieldset>
            <input class="form-control" type="text" align="left" aria-label="input example"  placeholder="ID" v-model="id">

            <div class="mb-3">
              <label for="requester" align="left" class="form-label">Requester</label>
              <input class="form-control" type="text" placeholder="Requester" id="requester" v-model="requester">
            </div>

            <div class="mb-3">
              <label for="project" class="form-label">Project</label>
              <input class="form-control" type="text" placeholder="Project" id="project" v-model="project">
            </div>

            <div class="mb-3">
              <label for="cluster" class="form-label">Cluster</label>
              <input class="form-control" type="text" placeholder="Cluster" id="cluster" v-model="cluster">
            </div>

            <div class="mb-3">
              <label for="database" class="form-label">Database</label>
              <input class="form-control" type="text" placeholder="Database" id="database" v-model="database">
            </div>

            <div class="mb-3">
              <label for="requestType" class="form-label">Request Type</label>
              <select type="list" placeholder="Request Type" id="requestType" v-model="requestType">
                <option default value="USER_ACCESS">USER_ACCESS</option>
                <option value="NETWORK_ACCESS">NETWORK_ACCESS</option>
                <option value="NEW_CLUSTER">NEW_CLUSTER</option>
              </select>
            </div>

            <input type="text" placeholder="Role" v-model="role">
            <input type="list" placeholder="Status" v-model="status">
            <input type="date" placeholder="Request Date" v-model="requestedDate">
          </fieldset>
          <div>
            <button class="btn button-primary" type="submit" value="Send">Submit</button>
          </div>
      </div>
    </div>
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