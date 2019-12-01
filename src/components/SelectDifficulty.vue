<template>
    <div class="select-difficulty">
        <div
            class="select-difficulty-container"
            :class="{'show-customize':isShowCustomize}"
        >
            <div class="select-difficulty-section">
                <div class="select-difficulty-row">
                    <div
                        class="select-difficulty-item"
                        @click="$emit('showGame',8,8,10)"
                    >
                        <div>8 x 8</div>
                        <div>10 个雷</div>
                    </div>
                    <div
                        class="select-difficulty-item"
                        @click="$emit('showGame',16,16,40)"
                    >
                        <div>16 x 16</div>
                        <div>40 个雷</div>
                    </div>
                </div>
                <div class="select-difficulty-row">
                    <div
                        class="select-difficulty-item"
                        @click="$emit('showGame',30,16,99)"
                    >
                        <div>30 x 16</div>
                        <div>99 个雷</div>
                    </div>
                    <div
                        class="select-difficulty-item"
                        @click="isShowCustomize = true"
                    >
                        <div>?</div>
                        <div>自定义</div>
                    </div>
                </div>
            </div>
            <div class="select-difficulty-section">
                <button @click="isShowCustomize = false">
                    www
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SelectDifficulty',
    data () {
        return {
            isShowCustomize: false,
            width: 30,
            height: 16,
            mineCount: 99,
        };
    },
};
</script>

<style scoped>
.select-difficulty {
    overflow: hidden;
}

.select-difficulty-container {
    display: flex;
    transition: transform 0.5s ease;
}

.select-difficulty-container.show-customize {
    transform: translateX(-100%);
}

.select-difficulty-section {
    width: 100%;
    flex: auto 0 0;
}

.select-difficulty-row {
    display: flex;
    justify-content: center;
}

.select-difficulty-item {
    width: 300px;
    height: 300px;
    margin: 15px;
    border: 2px solid #666;
    border-radius: 8px;
    text-align: center;
    padding-top: 120px;
}
</style>
