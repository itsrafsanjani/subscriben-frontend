<template>
  <div class="container flex flex-wrap pt-4 pb-10 m-auto mt-6 md:mt-15 lg:px-12 xl:px-16">
      <div class="w-full px-0 lg:px-4">
        <h2 class="px-12 text-base font-bold text-center md:text-2xl text-blue-700">
          Choose your plan
        </h2>
        <p class="py-1 text-sm text-center text-blue-700 mb-10">
          It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.
        </p>
        <div class="flex flex-wrap items-center justify-center py-4 pt-0">
          <!-- Plan Card -->
          <div class="w-full p-4 md:w-1/2 lg:w-1/4 plan-card" v-for="plan in plans" :key="plan.id">
            <label class="flex flex-col rounded-lg shadow-lg group relative cursor-pointer hover:shadow-2xl">
              <div class="w-full px-4 py-6 rounded-t-lg card-section-1">
                <h3 class="mx-auto text-base font-semibold text-center underline text-blue-500">
                  {{ plan.name }}
                </h3>
                <p class="text-5xl font-bold text-center text-blue-500">
                  {{ formatCurrency(plan.price) }}
                </p>
                <p class="text-xs text-center uppercase text-blue-500">
                  {{ plan.description }}
                </p>
              </div>
              <div
                class="flex flex-col items-center justify-center w-full h-full py-6 rounded-b-lg bg-blue-500"
              >
                <p class="text-xl text-white">
                  {{ plan.description }}
                </p>
                <NuxtLink :to="`plans/${plan.slug}`" class="w-5/6 py-2 mt-2 font-semibold text-center uppercase bg-white border border-transparent rounded text-blue-500">
                  Get Started
                </NuxtLink>
              </div>
            </label>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Plans',

  methods: {
      formatCurrency(price) {
          return price.toLocaleString('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 0});
      }
  },

  data() {
      return {
          plans: []
      }
  },

  created() {
        this.$axios.get('/api/v1/plans')
            .then(response => {
              this.plans = response.data.data
                console.log(response.data)
            })
            .catch(error => {
                console.log(error)
            });
    },
}
</script>

<style>

</style>
