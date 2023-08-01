<template>
  <!-- ... -->

  <!-- List of campaigns -->
  <!-- Form for creating or updating a campaign -->
  <form @submit.prevent="isUpdating ? updateCampaign : createCampaign">
    <!-- ... -->
    <button type="submit">{{ isUpdating ? "Update" : "Create" }} Campaign</button>
    <!-- Button to cancel the update if we're updating -->
    <button v-if="isUpdating" type="button" @click="cancelUpdate">Cancel</button>
  </form>

  <div v-for="campaign in campaigns" :key="campaign.id">
    <h2>{{ campaign.name }}</h2>
    <p>Scheduled Time: {{ campaign.scheduled_time }}</p>
    <p>Target Segment: {{ campaign.target_segment }}</p>
    <p>Email Body: {{ campaign.email_body }}</p>
    <button @click="editCampaign(campaign)">Edit</button>
    <button @click="deleteCampaign(campaign.id)">Delete</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      // ...
      selectedCampaign: null,
      campaigns: [],
    };
  },
  computed: {
    isUpdating() {
      return !!this.selectedCampaign;
    },
  },
  methods: {
    async createCampaign() {
      await axios.post("http://localhost:8000/api/email-campaigns", this.newCampaign);
      await this.fetchCampaigns();
      this.newCampaign = {
        name: "",
        scheduled_time: "",
        target_segment: "",
        email_body: "",
      };
    },
    editCampaign(campaign) {
      this.newCampaign = { ...campaign };
      this.selectedCampaign = campaign;
    },
    async updateCampaign() {
      await axios.put(`"http://localhost:8000/api/email-campaigns/${this.selectedCampaign.id}`, this.newCampaign);
      await this.fetchCampaigns();
      this.newCampaign = {
        name: "",
        scheduled_time: "",
        target_segment: "",
        email_body: "",
      };
      this.selectedCampaign = null;
    },
    cancelUpdate() {
      this.newCampaign = {
        name: "",
        scheduled_time: "",
        target_segment: "",
        email_body: "",
      };
      this.selectedCampaign = null;
    },

    async fetchCampaigns() {
      const response = await axios.get("http://localhost:8000/api/email-campaigns");

      this.campaigns = response.data;
    },

    async deleteCampaign(id) {
      await axios.delete(`"http://localhost:8000/api/email-campaigns/${id}`);
      await this.fetchCampaigns();
    },
  },
  async mounted() {
    await this.fetchCampaigns();
  },
};
</script>
