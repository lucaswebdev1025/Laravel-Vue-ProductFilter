<template>
    <dashboard-layout>
     <v-card class="mx-auto px-15 py-15">
        <v-card-text>
            <v-row class="mb-5">
                <v-col  v-if="activePricePlan && activePricePlan.name">
                    Your Plan is: {{activePricePlan.name}}
                </v-col>
                <v-col class="text-right">
                    <v-btn :to="{name: 'dashboard.billing.select-plan'}">Select Plan</v-btn>
                </v-col>
            </v-row>
            <stripe-checkout-card @submit-card="savePaymentMethod" />


            <div class="mt-15">
                <div  v-if="paymentMethods.length === 0" class="">
                    No payment method found, please add a payment method.
                </div>
                <div v-else>
                    <div v-for="(method, key) in paymentMethods"
                         :key="'method-'+key"
                         @click="paymentMethodSelected = method.id"
                         class="border rounded row p-1"
                         v-bind:class="{
                            'bg-success text-light': paymentMethodSelected == method.id
                        }">
                        <div class="col-2">
                            {{ method.brand.charAt(0).toUpperCase() }}{{ method.brand.slice(1) }}
                        </div>
                        <div class="col-7">
                            Ending In: {{ method.last_four }} Exp: {{ method.exp_month }} / {{ method.exp_year }}
                        </div>
                        <div class="col-3">
                            <span v-on:click.stop="removePaymentMethod( method.id )">Remove</span>
                        </div>
                    </div>
                </div>
            </div>
        </v-card-text>

    </v-card>
    </dashboard-layout>
</template>

<script>
    import BillingApi from "../../../api/BillingApi";
    import StripeCheckoutCard from "../../../components/StripeCheckoutCard";
    import { mapMutations} from "vuex";
    import PricePlan from "../../../models/PricePlan";
    import DashboardLayout from "../../../layouts/dashboard/Index";

    export default {
        name: "Index",
        components: {StripeCheckoutCard, DashboardLayout},
        data: function () {
            return {
                paymentMethods: [],
                paymentMethodSelected: {},
                activePricePlan: {},
            }
        },
        methods: {

            loadPaymentMethods(){
                window.axios.get('/api/billing/payment-methods').then(response => this.paymentMethods = response.data);
            },
            removePaymentMethod( methodStripeId ){
                BillingApi.removePaymentMethod(methodStripeId).then(() => {
                    this.loadPaymentMethods()
                    this.showSnackBar({color: 'success', timeout: 3000, text: 'Billing Method Removed Successfully'})
                })
            },
            savePaymentMethod( method ){
                window.axios.post('/api/billing/payments', {
                    payment_method: method
                }).then( () => {
                    this.showSnackBar({color: 'success', timeout: 3000, text: 'Billing Method Added Successfully'})
                    this.loadPaymentMethods()
                })
            },
            ...mapMutations(['showSnackBar']),
        },
        async mounted() {
            this.loadPaymentMethods();
            if (this.$auth.user().price_plan_id) {
                this.activePricePlan = await PricePlan.find(this.$auth.user().price_plan_id)
            }
        }
    }
</script>

<style scoped>

</style>
