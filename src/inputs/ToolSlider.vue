<style>

</style>

<template>
    <v-row>
        <v-col class="col-auto"><v-icon @click="decrement">mdi-minus</v-icon></v-col>
        <v-col class="col"><v-slider v-model="value" thumb-label="always" :label="label" :min="min" :max="max" @change="sendCmd" hide-details></v-slider></v-col>
        <v-col class="col-auto"><v-icon @click="increment">mdi-plus</v-icon></v-col>
    </v-row>
</template>


<script>
    export default {
        data: function() {
            return {
                value: this.target * this.multi,
                variableMax: 0,
            }
        },
        props: {
            target: {
                type: Number,
                required: true,
            },
            command: {
                type: String,
                required: true,
            },
            attributeName: {
                type: String,
                required: false,
                default: ''
            },
            label: {
                type: String,
                required: false,
                default: ''
            },
            attributeScale: {
                type: Number,
                required: false,
                default: 1
            },
            min: {
                type: Number,
                required: false,
                default: 0
            },
            max: {
                type: Number,
                required: false,
                default: 100
            },
            extenderSteps: {
                type: Number,
                required: false,
                default: 100
            },
            multi: {
                type: Number,
                required: false,
                default: 1
            },
        },
        computed: {

        },
        methods: {
            sendCmd() {
                this.$socket.sendObj('post_printer_gcode_script', { script: this.command+' '+this.attributeName+(this.value*this.attributeScale).toFixed(0) });
            },
            decrement() {
                this.value--;
                this.sendCmd();
            },
            increment() {
                this.value++;
                this.sendCmd();
            },
        },
        watch: {
            target: function() {
                this.value = this.target * this.multi;
            },
            /*value: function() {
                if (this.value > 0) {
                    if (this.value > (this.variableMax - this.extenderSteps) && this.value < (this.variableMax + this.extenderSteps)) this.variableMax += this.extenderSteps;
                    else if (this.value > (this.variableMax - this.extenderSteps)) {
                        this.variableMax = (this.value / this.extenderSteps).toFixed(0) * this.extenderSteps;
                        if (this.variableMax < this.max) this.variableMax = this.max;
                    }
                }
            }*/
        },
        /*created: function() {
            /!*window.console.log("Test");
            window.console.log(this.target+' * '+this.multi);
            this.value = this.target * this.multi;*!/
            window.console.log(this.target+' * '+this.multi);
            this.variableMax = (this.value / this.extenderSteps + 1).toFixed(0) * this.extenderSteps;
            if (this.variableMax < this.max) this.variableMax = this.max;
        }*/
    }
</script>