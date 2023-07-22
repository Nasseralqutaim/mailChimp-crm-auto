<template>
  <!-- ... -->

  <!-- List of campaigns -->
  <div v-for="campaign in campaigns" :key="campaign.id">
    <!-- Campaign details will go here -->
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
