<template>
    <div class="game-container">
        <div
            class="mine-sweeper-container"
            @contextmenu.prevent
        >
            <div
                v-for="i in height"
                :key="i"
                class="mine-sweeper-row"
            >
                <div
                    v-for="j in width"
                    :key="j"
                    class="mine-sweeper-item"
                    :class="{'is-open':openStatus[(i - 1) * width + j - 1]}"
                    @click.left="handleLeftClick(i-1,j-1)"
                    @click.right="handleRightClick(i-1,j-1)"
                >
                    <span
                        v-if="markStatus[(i-1)*width+(j-1)] === 1"
                        class="iconfont"
                    >&#xe778;</span>
                    <span
                        v-else-if="markStatus[(i-1)*width+(j-1)] === 2"
                        class="iconfont"
                    >&#xe720;</span>
                    <template v-else-if="openStatus[(i-1)*width+(j-1)]">
                        <span
                            v-if="mines[(i-1)*width+(j-1)]"
                            class="iconfont"
                        >&#xe63a;</span>
                        <span v-else-if="neighbourMineCount[(i-1)*width+(j-1)]>0">
                            {{ neighbourMineCount[(i-1)*width+(j-1)] }}
                        </span>
                    </template>
                </div>
            </div>
        </div>
        <div class="panel-container">
            <div class="panel-data-container">
                <span
                    class="iconfont"
                    style="font-size: 60px;"
                >&#xe778;</span>
                <div>
                    {{ selectedMineCount }} / {{ mineCount }}
                </div>
            </div>
            <div>
                <button
                    class="mine-sweeper-button"
                    @click="reStart"
                >
                    重开一局
                </button>

                <button
                    class="mine-sweeper-button"
                    style="margin-top: 15px;"
                    @click="$emit('selectDifficulty')"
                >
                    改变难度
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            width: 0,
            height: 0,
            mineCount: 0,
            isEnd: false,
            mines: [],
            openStatus: [],
            markStatus: [],
        };
    },
    computed: {
        neighbourMineCount () {
            const result = new Array(this.width * this.height).fill(0);
            for (let i = 0; i < result.length; i++) {
                if (this.mines[i]) {
                    continue;
                }
                const y = i % this.width;
                const x = (i - y) / this.width;
                for (let j = -1; j < 2; j++) {
                    const newX = x + j;
                    if (newX < 0 || newX === this.height) {
                        continue;
                    }
                    for (let k = -1; k < 2; k++) {
                        const newY = y + k;
                        if (newY < 0 || newY === this.width) {
                            continue;
                        }
                        if (this.mines[newX * this.width + newY]) {
                            result[i]++;
                        }
                    }
                }
            }
            return result;
        },
        selectedMineCount () {
            let count = 0;
            for (let i = 0; i < this.markStatus.length; i++) {
                if (this.markStatus[i] === 1) {
                    count++;
                }
            }
            return count;
        },
    },
    watch: {
        selectedMineCount () {
            this.$emit('updateSelectedMineCount', this.selectedMineCount);
            if (this.selectedMineCount === this.mineCount) {
                const match = this.mines.every((isMine, index) => {
                    if ((isMine && this.markStatus[index] === 1) || (!isMine && this.markStatus[index] !== 1)) {
                        return true;
                    }
                    return false;
                });
                if (match) {
                    this.$nextTick(() => {
                        alert('win');
                    });
                    this.isEnd = true;
                }
            }
        },
    },
    methods: {
        reStart () {
            this.init(this.width, this.height, this.mineCount);
        },
        init (width, height, mineCount) {
            this.width = width;
            this.height = height;
            this.mineCount = mineCount;
            this.isEnd = false;
            const total = width * height;
            const mines = new Array(total).fill(0);
            for (let i = 0; i < mineCount; i++) {
                mines[i] = 1;
            }
            this.shuffle(mines);
            this.mines = mines;
            this.openStatus = new Array(total).fill(0);
            this.markStatus = new Array(total).fill(0);
        },
        shuffle (mines) {
            for (let i = 1; i < mines.length; i++) {
                const randomIndex = Math.floor(Math.random() * (i + 1));
                const tmp = mines[randomIndex];
                mines[randomIndex] = mines[i];
                mines[i] = tmp;
            }
        },
        handleLeftClick (x, y) {
            if (this.isEnd) {
                return;
            }
            const index = x * this.width + y;
            if (this.openStatus[index] === 1 || this.markStatus[index] === 1) {
                return;
            }

            if (this.mines[index]) {
                this.openStatus.splice(index, 1, 1);
                this.isEnd = true;
                this.$nextTick(() => {
                    alert('mine');
                });

                return;
            }
            if (this.neighbourMineCount[index] > 0) {
                this.openStatus.splice(index, 1, 1);
                return;
            }
            this.floodfill(x, y);
        },
        floodfill (x, y) {
            if (x < 0 || y < 0 || x === this.height || y === this.width) {
                return;
            }
            const index = x * this.width + y;
            if (this.openStatus[index] === 1) {
                return;
            }
            this.openStatus.splice(index, 1, 1);
            if (this.neighbourMineCount[index] > 0) {
                return;
            }
            for (let i = -1; i < 2; i++) {
                for (let j = -1; j < 2; j++) {
                    this.floodfill(x + i, y + j);
                }
            }
        },
        handleRightClick (x, y) {
            if (this.isEnd) {
                return;
            }
            const index = x * this.width + y;
            if (this.openStatus[index] === 1) {
                return;
            }
            this.markStatus.splice(index, 1, (this.markStatus[index] + 1) % 3);
        },
    },
};
</script>

<style scoped>
.game-container {
    display: flex;
    justify-content: space-between;
    padding: 0 15px;
}

.mine-sweeper-container {
    overflow: hidden;
    width: fit-content;
    margin: 0 auto;
    background-color: #f2f1f0;
}

.mine-sweeper-row {
    display: flex;
}

.mine-sweeper-item {
    width: 50px;
    height: 50px;
    margin: 2px;
    line-height: 50px;
    font-size: 34px;
    text-align: center;
    background-color: #babdb6;
    cursor: pointer;
}

.mine-sweeper-item.is-open {
    background-color: #dededc;
}

.panel-container {
    width: fit-content;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.panel-data-container {
    padding-top: 15px;
    text-align: center;
}

</style>
