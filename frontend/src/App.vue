<template>
    <v-app>
        <router-view/>
        <saas-snackbar />
    </v-app>
</template>

<script>
    import SaasSnackbar from "./components/SaasSnackbar";

    export default {
        name: 'App',
        computed: {
            layout() {
                return `${(this.$route.meta.layout || 'empty')}-layout`
            },
        },
        components: {
            SaasSnackbar,
        },

        data: () => ({
            //
        }),
    };
</script>
<style type="text/css">
    #nprogress .bar {
        height: 4px !important;
    }

    .v-time-picker-custom .v-picker__title {
        height: 84px;
        padding-top: 10px;
    }
</style>
