<template>
  <div class="w-full">
    <div
      class="fixed text-5xl text-right right-4 top-2 md:text-7xl lg:text-8xl xl:text-9xl"
    >
      <div class="flex justify-end space-x-2">
        <span class="font-thin text-opacity-100 text-fuchsia-600">{{
          days
        }}</span>
        <span class="font-extrabold uppercase text-opacity-80 text-fuchsia-600"
          >Days</span
        >
      </div>
      <div class="flex justify-end space-x-2">
        <span class="font-thin text-opacity-100 text-fuchsia-600">{{
          hours
        }}</span>
        <span class="font-extrabold uppercase text-opacity-80 text-fuchsia-600"
          >Hours</span
        >
      </div>
      <div class="flex justify-end space-x-2">
        <span class="font-thin text-opacity-100 text-fuchsia-600">{{
          minutes
        }}</span>
        <span class="font-extrabold uppercase text-opacity-80 text-fuchsia-600"
          >Minutes</span
        >
      </div>
      <div class="flex justify-end space-x-2">
        <span class="font-thin text-opacity-100 text-fuchsia-600">{{
          seconds
        }}</span>
        <span class="font-extrabold uppercase text-opacity-80 text-fuchsia-600"
          >Seconds</span
        >
      </div>
    </div>
    <div class="flex flex-wrap items-start justify-start w-full">
      <span
        v-for="index in totalMinutes"
        :key="index"
        class="block w-2 h-1 mt-px ml-px bg-fuchsia-600 xl:w-3 xl:h-1.5"
        :class="[isFuture(index) ? 'bg-opacity-10' : 'bg-opacity-50']"
      >
      </span>
      <span
        class="block w-[26px] h-1 mt-px ml-px bg-opacity-50 bg-white xl:w-[38px] xl:h-1.5 relative"
      >
        <span
          class="absolute flex items-baseline text-xs uppercase left-1 top-3 text-fuchsia-400"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="w-4 h-4"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M8 7l4-4m0 0l4 4m-4-4v18"
            />
          </svg>
          <span>Vacation</span>
        </span>
      </span>
    </div>
    <div class="px-4 py-4 mt-8 text-right md:text-left">
      <p class="text-xs text-fuchsia-400">1 block equals 1 minute</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CountdownTimer',
  data() {
    return {
      interval: null,
      countdownDate: new Date('April 16, 2022').getTime(),
      startDate: new Date('April 6, 2022').getTime(),
      distance: 0,
      expired: false,
    }
  },

  computed: {
    days() {
      return Math.floor(this.distance / (1000 * 60 * 60 * 24))
    },
    hours() {
      return Math.floor(
        (this.distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
      )
    },
    minutes() {
      return Math.floor((this.distance % (1000 * 60 * 60)) / (1000 * 60))
    },
    seconds() {
      return Math.floor((this.distance % (1000 * 60)) / 1000)
    },
    totalDistance() {
      return this.countdownDate - this.startDate
    },

    totalMinutes() {
      return Math.floor(this.totalDistance / 1000 / 60)
    },

    currentMinutes() {
      return this.totalMinutes - Math.floor(this.distance / 1000 / 60)
    },
  },

  beforeMount() {
    this.interval = setInterval(this.countdownTick, 1000)
  },

  methods: {
    countdownTick() {
      const now = new Date().getTime()

      // Find the distance between now and the count down date
      this.distance = this.countdownDate - now

      // If the count down is finished, write some text
      if (this.distance < 0) {
        clearInterval(this.interval)
        this.expired = true
      }
    },
    isFuture(i) {
      return this.currentMinutes < i
    },
  },
}
</script>
