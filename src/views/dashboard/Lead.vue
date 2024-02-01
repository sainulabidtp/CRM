<template>
  <div class="container">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">{{ lead.company }}</h1>

        <div class="buttons">
         <router-link :to="{ name: 'EditLead', params: { id: lead.id }}" class="button is-light">Edit</router-link>
          <button class="button is-info">Convert to client</button>
          <button class="button is-danger">Delete</button>
        </div>
      </div>

      <div class="column is-6">
        <div class="box">
          <h2 class="subtitle">Details</h2>

          <template>
            <p><strong>Assigned to: </strong>{{ lead.status }}</p>
          </template>
          <p><strong>Status: </strong>{{ lead.status }}</p>
          <p><strong>Priority: </strong>{{ lead.priority }}</p>
          <p><strong>Confidence: </strong>{{ lead.confidence }}</p>
          <p><strong>Estimated value: </strong>{{ lead.estimated_value }}</p>
          <p><strong>Created at: </strong>{{ lead.created_at }}</p>
          <p><strong>Modified at: </strong>{{ lead.modified_at }}</p>
        </div>
      </div>

      <div class="column is-6">
        <div class="box">
          <h2 class="subtitle">Contact information</h2>

          <p><strong>Contact person: </strong>{{ lead.contact_person }}</p>
          <p><strong>Email: </strong>{{ lead.email }}</p>
          <p><strong>Phone: </strong>{{ lead.phone }}</p>
          <p><strong>Website: </strong>{{ lead.website }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Lead',
  data() {
    return {
      lead: {},
    };
  },
  mounted() {
    this.getLead();
  },
  methods: {
    async getLead() {
  this.$store.commit('setIsLoading', true);

  const leadID = this.$route.params.id;

  try {
    const response = await axios.get(`http://127.0.0.1:8000/api/v1/leads/${leadID}/`, this.$store.state.token );
    // Handle successful response
    this.lead = response.data; // Remove [0] if response.data is an array
    console.log("ssss", this.lead);
    console.log(response.data);
  } catch (error) {
    // Handle errors, including authentication errors
    console.error(error.message);
  }

  this.$store.commit('setIsLoading', false);
}

  },
};
</script>
