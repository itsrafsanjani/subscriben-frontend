<template>
    <BreezeAuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                {{ $route.params.slug }}
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 bg-white border-b border-gray-200">
                      <stripe-element-card
                        ref="elementRef"
                        :pk="publishableKey"
                        :elements-options="elementsOptions"
                      />
                      <button @click="pay">Generate token</button>
                    </div>
                </div>
            </div>
        </div>
    </BreezeAuthenticatedLayout>
</template>

<script>
import BreezeAuthenticatedLayout from '@/layouts/authenticated.vue'

import { StripeElementCard } from '@vue-stripe/vue-stripe';
export default {
    head: {
        title: 'Dashboard',
    },

    middleware: 'authenticated',

    data () {
      this.publishableKey = process.env.stripeKey;
      return {
        token: null,
        plan_slug: null,
        plan: null,
        paymentMethods: [],
        intent: null,
        elementsOptions: {
          appearance: {}, // appearance options
        },
      };
    },

    mounted () {
      this.plan_slug = this.$route.params.slug

      this.$axios.get('/api/v1/plans/' + this.plan_slug)
        .then(response => {
          console.log(response.data)
          this.plan = response.data.plan
          this.paymentMethods = response.data.paymentMethods
          this.intent = response.data.intent
          this.elementsOptions.clientSecret = response.data.intent.client_secret;
        })
        .catch(error => {
          console.log(error)
        })
    },

    methods: {
      pay () {
        this.$refs.paymentRef.submit();
        this.$axios.post('/api/v1/subscriptions', {
          plan: this.plan_slug,
          paymentMethod: this.paymentMethods,
        })
          .then(response => {
            console.log(response.data)
          })
          .catch(error => {
            console.log(error)
          });
      },
  },

    components: {
        BreezeAuthenticatedLayout,
        StripeElementCard
      },
    }
</script>
