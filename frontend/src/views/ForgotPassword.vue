<template>
    <front-layout>
        <div class="flex-wrapper-full-background d-flex  fill-height" style="width: 100%">
            <v-card class="mx-auto my-auto pa-5" max-width="900" min-width="500" raised>
                <v-card-title>{{$t('scoliotracker.password_reset')}}</v-card-title>
                <v-card-text>
                    <v-form>
                        <v-row>
                            <v-col>
                                <v-text-field  :error-messages="errors.email" :label="$t('scoliotracker.email')" v-model="email"/>
                            </v-col>
                        </v-row>

                    </v-form>
                </v-card-text>
                <v-card-actions  class="text-center">
                    <v-btn @click="handleForgotPassword">{{$t('scoliotracker.reset')}}</v-btn>
                </v-card-actions>
            </v-card>
        </div>
    </front-layout>
</template>

<script>
    import {mapActions, mapMutations} from "vuex";
    import FrontLayout from "../layouts/FrontLayout";
    import Api from "../api/Api";

    export default {
        name: "ForgotPassword",
        components: {FrontLayout},
        data: function () {
            return {
                email: '',
                errors: {}
            }
        },
        methods: {
            async handleForgotPassword() {
                const {email} = this;
                let response = await Api.forgotPassword(email).catch(e => this.errors = e.response.data.errors)
                if (response.status === 200) {
                    this.errors = {}
                    this.showSnackBar({color: 'success', timeout: 3000, text: 'Password Reset Confirmed'})
                }
            },
            ...mapActions(['signUp']),
            ...mapMutations(['showSnackBar']),
        },
    }
</script>

<style scoped lang="scss">

</style>
