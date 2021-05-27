<template>
  <div>
    <request-item v-for="userRequest in userRequests" :key="userRequest.id" :userRequest="userRequest">
      {{userRequest.id}}
      {{userRequest.requester}}
      {{userRequest.requestType}}
      {{userRequest.requestedDate}}
      {{userRequest.status}}
    </request-item>
  </div>
</template>

<script>
import RequestItem from "@/components/RequestItem";
import gql from "graphql-tag";

const GET_REQUESTS = gql`
  query getRequests {
    listUserRequests {
      id
      requester
      project
      cluster
    }
  }
`;

export default {
  name: "RequestList",
  components: {RequestItem},
  comments: { RequestItem },
  data() {
    return {

      userRequests: []

      // userRequests: [
      //   {
      //     "id": "1000",
      //     "requester": "Rajesh R",
      //     "project": "my project",
      //     "cluster": "cluster 0",
      //     "database": "reports",
      //     "requestType": "USER_ACCESS",
      //     "role": "user",
      //     "status": "OPEN",
      //     "requestedDate": "05-May-2021"
      //   }
      // ]

    };
  },
  apollo: {
    userRequests: {
      query: GET_REQUESTS
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>