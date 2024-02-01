<template>
  <div class="container">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">Edit {{ lead.company }}</h1>
      </div>

      <div class="column is-12">
        <form @submit.prevent="submitForm">
          <div class="field">
            <label>Company</label>
            <div class="control">
              <input type="text" class="input" v-model="lead.company" />
            </div>
          </div>

          <div class="field">
            <label>Contact person</label>
            <div class="control">
              <input type="text" class="input" v-model="lead.contact_person" />
            </div>
          </div>

          <div class="field">
            <label>Email</label>
            <div class="control">
              <input type="email" class="input" v-model="lead.email" />
            </div>
          </div>

          <div class="field">
            <label>Phone</label>
            <div class="control">
              <input type="text" class="input" v-model="lead.phone" />
            </div>
          </div>

          <div class="field">
            <label>Website</label>
            <div class="control">
              <input type="text" class="input" v-model="lead.website" />
            </div>
          </div>

          <div class="field">
            <label>Confidence</label>
            <div class="control">
              <input type="number" class="input" v-model="lead.confidence" />
            </div>
          </div>

          <div class="field">
            <label>Estimated value</label>
            <div class="control">
              <input
                type="number"
                class="input"
                v-model="lead.estimated_value"
              />
            </div>
          </div>

          <div class="field">
            <label>Status</label>
            <div class="control">
              <div class="select">
                <select v-model="lead.status">
                  <option value="new">New</option>
                  <option value="contacted">Contacted</option>
                  <option value="inprogress">In progress</option>
                  <option value="lost">Lost</option>
                  <option value="won">Won</option>
                </select>
              </div>
            </div>
          </div>

          <div class="field">
            <label>Priority</label>
            <div class="control">
              <div class="select">
                <select v-model="lead.priority">
                  <option value="low">Low</option>
                  <option value="medium">Medium</option>
                  <option value="high">High</option>
                </select>
              </div>
            </div>
          </div>

          <div class="field">
            <label>Assigned to</label>
            <div class="control">
              <div class="select">
                <select v-model="lead.assigned_to">
                  <option value="" selected>Select member</option>
                  <option></option>
                </select>
              </div>
            </div>
          </div>

          <div class="field">
            <div class="control">
              <button class="button is-success">Update</button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

import { toast } from "bulma-toast";

export default {
  name: "EditLead",
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
      this.$store.commit("setIsLoading", true);

      const leadID = this.$route.params.id;

      try {
        const response = await axios.get(
          `http://127.0.0.1:8000/api/v1/leads/${leadID}/`,
          this.$store.state.token
        );
        // Handle successful response
        this.lead = response.data; // Remove [0] if response.data is an array
        console.log("ssss", this.lead);
        console.log(response.data);
      } catch (error) {
        // Handle errors, including authentication errors
        console.error(error.message);
      }

      this.$store.commit("setIsLoading", false);
    },
    async submitForm() {
      this.$store.commit("setIsLoading", true);

      const leadID = this.$route.params.id;

      axios
        .patch(`/api/v1/leads/${leadID}/`, this.lead)
        .then((response) => {
          toast({
            message: "The lead was updated",
            type: "is-success",
            dismissible: true,
            pauseOnHover: true,
            duration: 2000,
            position: "bottom-right",
          });

          this.$router.push(`/dashboard/leads/${leadID}`);
        })
        .catch((error) => {
          console.log(error);
        });

        

      this.$store.commit("setIsLoading", false);
    },
  },
};
</script>