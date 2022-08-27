<script setup></script>

<template>
  <div class=" ">
    <div
      class="bg-teal-600 h-36 bg-[url('./assets/bg-header-desktop.svg')] relative"
    >
      <div
        v-if="showBar"
        class="lg:w-1/2 w-72 shadow lg:h-12 py-2 px-6 absolute top-32 left-1/4 grid grid-cols-10 rounded bg-white"
      >
        <div class="col-span-8 flex flex-wrap items-center">
          <div
            class="bg-[#E9F5F3] mt-2 lg:mt-0 text-xs text-[#729C9B] font-bold rounded px-2 py-1 mx-1 flex items-center"
            v-for="(clip, index) in clips"
            :key="index"
          >
            <div class="flex items-center">
              {{ clip }}
              <button @click="clearThis(clip)">
                <img class=" h-4 m-1 p-1 bg-[#5BA5A1] rounded
                hover:bg-slate-800" src=./assets/icon-remove.svg>
              </button>
            </div>
          </div>
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

    <div
      class="grid grid-flow-row justify-items-center gap-12 lg:gap-4 lg:mt-20 mt-36 py-8"
    >
      <div
        v-for="(job, index) in jobs"
        :key="index"
        class="bg-white px-8 py-6 rounded-md lg:w-[900px] w-72"
        :class="[job.featured ? 'featured' : '']"
      >
        <div class="grid lg:grid-cols-7 grid-cols-1 gap-4 relative">
          <div class="col-span-1">
            <img
              class="w-16 absolute -top-14 -left-2"
              :src="job.img"
              :alt="job.company"
            />
          </div>
          <div class="col-span-3 grid grid-flow-row content-center gap-2">
            <div class="flex gap-2">
              <h3 class="text-sm text-[#729C9B] font-bold">
                {{ job.company }}
              </h3>
              <div
                v-if="job.new"
                class="bg-[#5ba5a1] px-2 py-1 font-semibold text-xs text-white rounded-xl uppercase"
              >
                New!
              </div>
              <div
                v-if="job.featured"
                class="bg-gray-800 px-2 py-1 font-semibold text-xs text-white rounded-xl uppercase"
              >
                Featured
              </div>
            </div>
            <h3 class="font-semibold text-gray-700">{{ job.title }}</h3>
            <div class="flex font-medium text-xs text-gray-500 gap-4">
              <p>{{ job.posted }}</p>
              <p>{{ job.type }}</p>
              <p>{{ job.location }}</p>
            </div>
          </div>
          <div
            class="border-t lg:border-none col-span-3 lg:flex lg:py-8 py-4 justify-end"
          >
            <button
              @click="jobFilter(tag)"
              class="bg-[#E9F5F3] text-xs text-[#729C9B] font-bold rounded px-2 py-1 mx-1 mt-2 hover:bg-[#5BA5A1] hover:text-white"
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
      showBar: false,
      // showBar: false,
    };
  },

  mounted() {
    this.jobs = jobs;
  },

  methods: {
    jobFilter(tag) {
      this.clips.push(tag);
      (this.showBar = true), (this.tag = tag);
      this.jobs = this.jobs.filter((job) => {
        return job.tags.includes(tag);
      });
    },

    resetJobs() {
      this.jobs = jobs;
      this.clips = [];
      this.tag = "";
      this.showBar = false;
    },

    clearThis(clip) {
      this.clips = this.clips.filter((data) => data != clip);
      this.jobs = jobs;
      this.clips.map((clip) => {
        this.jobs = this.jobs.filter((job) => {
          return job.tags.includes(clip);
        });
      });

      if (this.clips.length == 0) {
        this.showBar = false;
      }
    },
  },
};
</script>

<style>
.featured {
  border-left: 4px solid #5ba5a1;
}
</style>
