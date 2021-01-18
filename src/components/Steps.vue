<template>
    <div class="steps">
        <hr/>
        <div class="step">
            <p class="title">{{allData.title}}:</p>

            <div v-for="(data, index) in allData.variants" v-bind:key="data">
                <Variant
                    v-bind:description="data"
                    v-bind:varId="index"
                    v-bind:registerCurrentVariant="registerCurrentVariant" />
            </div>

            
        </div>
    </div>
</template>


<script>
import Variant from './Variant.vue'

export default {
    name: "Steps",
    props: {
        allData: Object,
        stepId: Number,
        confirmTotalPrice: Function,
    },
    components: {
        Variant
    },
    methods: {
        registerCurrentVariant: function (price) {
            this.confirmTotalPrice(price, this.stepId);
        }
    }
}
</script>