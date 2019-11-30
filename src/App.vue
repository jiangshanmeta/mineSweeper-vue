<template>
    <div id="app">
        <MineSweeper
            ref="mineSweeper"
        />
    </div>
</template>

<script>
import MineSweeper from '@/components/MineSweeper';
export default {
    name: 'App',
    components: {
        MineSweeper,
    },
    data () {
        return {};
    },
    mounted () {
        this.$nextTick(() => {
            this.$refs.mineSweeper.init(30, 10, 10);
        });
    },
};
</script>
