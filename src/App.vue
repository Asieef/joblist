<script setup></script>

<template>
  <div class="bg-[#F0FAFB] h-fit">
    <div class="relative">
      <img class="" src="./assets/bg-header-desktop.svg" />
      <div
        class="absolute w-[800px] shadow h-12 left-60 bg-white top-28 px-6 grid grid-cols-10 rounded"
      >
        <div class="col-span-8 flex items-center">
          <button
            class="bg-[#E9F5F3] text-xs text-[#729C9B] font-bold rounded px-2 py-1 mx-1 flex items-center"
            v-for="(clip, index) in clips"
            :key="index"
          >
            <button @click="clearThis(clip)">
              {{ clip }} <img @click="removeJobs" class=" h-4 m-1 p-1
              bg-[#5BA5A1] rounded hover:bg-slate-800"
              src=./assets/icon-remove.svg>
            </button>
          </button>
        </div>
        <div class="col-span-2 grid content-center">
          <button
            @click="resetJobs"
            class="text-xs text-[#729C9B] font-bold hover:underline"
          >
            clear
          </button>
        </div>
      </div>
    </div>

    <div class="grid grid-flow-row justify-items-center gap-4 mt-20 py-8">
      <div
        v-for="(job, index) in filteredJobs"
        :key="index"
        class="bg-white px-8 py-6 rounded-md w-[900px]"
      >
        <div class="grid grid-cols-7 gap-4">
          <div class="col-span-1">
            <img :src="job.img" :alt="company" />
          </div>
          <div class="col-span-3 grid grid-flow-row content-center">
            <h3 class="text-sm text-[#729C9B] font-bold">
              {{ job.company }}
            </h3>
            <h3 class="font-semibold text-gray-700">{{ job.title }}</h3>
            <div class="flex font-medium text-xs text-gray-500 gap-4">
              <p>{{ job.posted }}</p>
              <p>{{ job.type }}</p>
              <p>{{ job.location }}</p>
            </div>
          </div>
          <div class="col-span-3 flex py-8 justify-end">
            <button
              @click="jobFilter(tag)"
              class="bg-[#E9F5F3] text-xs text-[#729C9B] font-bold rounded px-2 py-1 mx-1 hover:bg-[#5BA5A1] hover:text-white"
              v-for="(tag, index) in job.tags"
              :key="index"
            >
              {{ tag }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import jobs from "../src/data/jobs.json";
export default {
  data() {
    return {
      jobs: [],
      clips: [],
      tag: "",
    };
  },

  mounted() {
    this.jobs = jobs;
  },

  computed: {
    filteredJobs() {
      if (this.tag) {
        return this.jobs.filter((job) => {
          return job.tags.includes(this.tag);
        });
      } else {
        return this.jobs;
      }
    },
  },

  methods: {
    jobFilter(tag) {
      console.log(tag);
      this.clips.push(tag);
      this.tag = tag;
      this.jobs = this.jobs.filter((job) => {
        return job.tags.includes(tag);
      });
    },

    resetJobs() {
      this.jobs = jobs;
      this.clips = [];
      this.tag = "";
    },

    clearThis(clip) {
      console.log(clip);
      this.clips = this.clips.filter((data) => data != clip);
      this.tag = "";
    },

    // removeJobs(tag) {
    //   this.jobs = this.jobs.filter((job) => {
    //     return job.tags.includes(tag);
    //   });
    // },
  },
};
</script>
