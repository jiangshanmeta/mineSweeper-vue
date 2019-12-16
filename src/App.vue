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
                :play="isShowGame"
                :width="width"
                :height="height"
                :mine-count="mineCount"
                class="app-section"
                @selectDifficulty="selectDifficulty"
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
            width: 0,
            height: 0,
            mineCount: 0,
        };
    },
    methods: {
        showGame (width, height, mineCount) {
            this.width = width;
            this.height = height;
            this.mineCount = mineCount;
            this.isShowGame = true;
        },
        selectDifficulty () {
            this.isShowGame = false;
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
    flex: 0 0 auto;
}
</style>
