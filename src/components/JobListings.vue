<script setup>
import { ref, defineProps, onMounted, reactive } from "vue";
import jobData from "@/jobs.json";
import JobListing from "@/components/JobListing.vue";
import axios from "axios";

const state = reactive({
  jobs: [],
  isLoading: true,
});

onMounted(async () => {
  try {
    const response = await axios.get("http://localhost:5000/jobs");
    state.jobs = response.data;
  } catch (error) {
    console.error("Error fetching jobs:", error);
  } finally {
    state.isLoading = false;
  }
});

defineProps({
  limit: Number,
  showButton: {
    type: Boolean,
    default: true,
  },
});
</script>

<template>
  <section class="bg-green-50 px-4 py-10">
    <div class="container-xl lg:container m-auto">
      <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
        Browse Jobs
      </h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <JobListing
          v-for="job in state.jobs.slice(0, limit || state.jobs.length)"
          :key="job.id"
          :job="job"
        />
        <!-- Job Listing 1 -->
      </div>
    </div>
  </section>

  <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
    <a
      href="/jobs/"
      class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
      >View All Jobs</a
    >
  </section>
</template>
