<template>
    <div class="input-group">
        <div
            class="input-group-addon"
            @click="dpMinus"
        >
            -
        </div>
        <input
            ref="input"
            v-model="model"
            class="form-control"
        >
        <div
            class="input-group-addon"
            @click="doPlus"
        >
            +
        </div>
    </div>
</template>

<script>
export default {
    props: {
        value: {
            type: Number,
            required: true,
        },
        min: {
            type: Number,
            default: -Infinity,
        },
        max: {
            type: Number,
            default: Infinity,
        },
        step: {
            type: Number,
            default: 1,
        },
    },
    computed: {
        model: {
            get () {
                return this.value;
            },
            set (val) {
                const numberVal = Number(val);
                if (Number.isNaN(numberVal) || !Number.isInteger(numberVal) || numberVal > this.max || numberVal < this.min) {
                    this.$refs.input.value = this.value;
                    return;
                }

                this.$emit('input', numberVal);
            },
        },
    },
    watch: {
        min: 'handleMinMaxChange',
        max: 'handleMinMaxChange',
    },
    methods: {
        dpMinus () {
            this.model = this.value - this.step;
        },
        doPlus () {
            this.model = this.value + this.step;
        },
        handleMinMaxChange () {
            if (this.value > this.max) {
                this.model = this.max;
            }
            if (this.value < this.min) {
                this.model = this.min;
            }
        },
    },
};
</script>

<style scoped>
.input-group {
    display: flex;
}

.input-group-addon {
    padding: 6px 12px;
    background-color: #eee;
    color: #555;
    font-size: 14px;
    text-align: center;
    border: 1px solid #ccc;
    border-radius: 4px;
    cursor: pointer;
}

.input-group-addon:first-child {
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
}

.input-group-addon:last-child {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
}

.form-control {
    padding: 6px 12px;
    font-size: 14px;
    line-height: 1.42;
    color: #555;
    background-color: #fff;
    background-image: none;
    border: 1px solid #ccc;
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
    outline: none;
}
</style>
