<template>
    <div class="app-container" style="height: 600px;overflow-y: auto;">
        <span v-for="prompt in prompts">
            <el-divider content-position="left">{{ prompt.name }}</el-divider>
            <span v-for="item in prompt.items" class="prompt">
                <el-button type="warning" v-if="setting.down" @click="downWeight" plain>
                    <el-icon><Remove /></el-icon>
                </el-button>
                <el-button type="warning" v-if="setting.up" @click="upWeight" plain>
                    <el-icon><CirclePlus /></el-icon>
                </el-button>
                <el-button @click="selectPrompt" plain>
                    <span v-if="setting.en">{{ item.en }}</span>
                    <span v-if="setting.zh">「{{ item.zh }}」</span>
                </el-button>
            </span>
        </span>
    </div>
</template>
  
<script>
import prompts from '../data/hand'

export default {
    name: 'Hand',
    props: {
        setting: Object
    },
    data() {
        return {}
    },
    created () {},
    methods: {
        downWeight($event) {
            let target = $event.currentTarget.parentElement.lastElementChild.firstElementChild.firstElementChild;
            if(target.innerText.startsWith('(') && target.innerText.endsWith(')')) {
                target.innerText = target.innerText.substring(1, target.innerText.length -1)
            } else {
                target.innerText = `[${target.innerText}]`
            }
        },
        upWeight($event) {
            let target = $event.currentTarget.parentElement.lastElementChild.firstElementChild.firstElementChild;
            if(target.innerText.startsWith('[') && target.innerText.endsWith(']')) {
                target.innerText = target.innerText.substring(1, target.innerText.length -1)
            } else {
                target.innerText = `(${target.innerText})`
            }
        },
        selectPrompt ($event) {
            let prompt = $event.currentTarget.firstElementChild.firstElementChild;
            this.$emit('updateSelect', prompt.innerText);
        }
    },
    computed: {
        prompts: function () {
            return this.setting.adult ? prompts : prompts.filter((item) => item.type !== 'adult');
        }
    }
}
</script>
  
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.el-button {
    margin-left: .2rem;
    margin-bottom: .5rem;
}

.prompt {
    margin: 2px;
}
</style>