<template>
    <div id="app">
        <div
            class="app-main"
            :class="{'show-game':isShowGame}"
        >
            <SelectDifficulty
                class="app-section"
                @showGame="showGame"
            />

            <MineSweeper
                ref="mineSweeper"
                class="app-section"
                @selectDifficulty="isShowGame = false"
            />
        </div>
    </div>
</template>

<script>
import SelectDifficulty from '@/components/SelectDifficulty';
import MineSweeper from '@/components/MineSweeper';

export default {
    name: 'App',
    components: {
        SelectDifficulty,
        MineSweeper,
    },
    data () {
        return {
            isShowGame: false,
        };
    },
    methods: {
        showGame (width, height, mineCount) {
            this.$refs.mineSweeper.init(width, height, mineCount);
            this.isShowGame = true;
        },
    },
};
</script>

<style scoped>
#app {
    display: flex;
    min-height: 100vh;
    overflow: hidden;
}

.app-main {
    width: 100%;
    display: flex;
    align-items: center;
    transition: transform 1s ease;
}

.app-main.show-game {
    transform: translateX(-100%);
}

.app-section {
    width: 100%;
    flex: auto 0 0;
}
</style>
