<template>
  <form @submit="submit" >
    <div class="container-fluid">
      <div align="center" >
        <img alt="Vue logo" src="../assets/mongodb-image.png" height="200" width="320"/>
        <h5>Create new request</h5>
      </div>
      <div class="row g-3" >
            <input class="form-control" type="text" align="left" aria-label="input example"  placeholder="ID" v-model="id">

            <div class="col-md-12">
              <label for="requester" align="left" class="form-label">Requester</label>
              <input class="form-control" type="email" placeholder="Requester" id="requester" v-model="requester">
            </div>

            <div class="col-md-4">
              <label for="project" class="form-label">Project</label>
              <input type="text" class="form-control" id="project" placeholder="Project" v-model="project">
            </div>
            <div class="col-md-4">
              <label for="cluster" class="form-label">Cluster</label>
              <input type="text" class="form-control" id="cluster" placeholder="Cluster" v-model="cluster">
            </div>
            <div class="col-md-4">
              <label for="database" class="form-label">Database</label>
              <input type="text" class="form-control" id="database" placeholder="Database" v-model="database">
            </div>
            <div class="col-md-4">
              <label for="requestType" class="form-label">Request Type</label>
              <select id="requestType" class="form-select" v-model="requestType" @change="displayInputFields">
                <option selected value="USER-ACCESS">USER-ACCESS</option>
                <option value="NETWORK-ACCESS">NETWORK-ACCESS</option>
                <option value="NEW-INFRA">NEW-INFRA</option>
              </select>
            </div>

            <div class="form-row" v-for="(inputField, index) in inputFields" :key="index">
              <div class="col-md-4">
                <label>{{ inputField.label }}</label>
                <input v-model="inputField.id" id="{inputFields[${index}][id]}" :name="`inputFields[${index}][label]`" type="text" class="form-control" :placeholder="`inputFields[${index}][label]`">
              </div>
            </div>

<!--            <input type="text" placeholder="Role" hidden v-model="role">-->
            <input type="list" placeholder="Status" v-model="status">
            <input type="date" placeholder="Request Date" v-model="requestedDate">

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
import { uuid } from 'vue-uuid' // Import uuid

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
  apollo: {},
  data() {
    return {
      inputFields: [
      ],
      id: "",
      requester: "",
      project: "",
      cluster: "",
      database: "",
      requestType: "",
      role: "",
      status: "",
      requestedDate: "",
    }
  },
  methods: {
    displayInputFields () {
      if (this.requestType === 'USER-ACCESS') {
        this.inputFields = [];
        this.inputFields.push({
          label: 'Role',
          id: 'role'
        })
      } else if (this.requestType === 'NETWORK-ACCESS') {
        this.inputFields = [];
        this.inputFields.push({
          label: 'CIDR/IP',
          id: 'cidr'
        })
      }
    },
    currentDateTime() {
      const current = new Date();
      const date = current.getFullYear()+'-'+(current.getMonth()+1)+'-'+current.getDate();
      const time = current.getHours() + ":" + current.getMinutes() + ":" + current.getSeconds();
      const dateTime = date +' '+ time;

      return dateTime;
    },
    submit(e) {
      e.preventDefault();
      const { requester, project, cluster, database, requestType } = this.$data;
      if (this.requestType === 'USER-ACCESS') {
        this.role = this.inputFields[0].id;
      }
      this.$apollo.mutate({
        mutation: ADD_REQUEST,
        variables: {
          id: uuid.v1(),
          requester,
          project,
          cluster,
          database,
          requestType,
          role: this.role,
          status: 'OPEN',
          requestedDate: this.currentDateTime()
        },
        refetchQueries: ["getRequests"]
      });
    }
  }
};
</script>