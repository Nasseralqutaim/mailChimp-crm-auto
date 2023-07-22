<template>
  <!-- ... -->

  <!-- List of campaigns -->
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
      campaigns: [],
    };
  },
  methods: {
    async fetchCampaigns() {
      const response = await axios.get("/api/email-campaigns");
      this.campaigns = response.data;
    },
    // ...
  },
  async mounted() {
    await this.fetchCampaigns();
  },
};
</script>

<!-- ... -->
