<template>
<li>
    <!-- 这里是单项的内容 -->
    <div
     :class="{bold: isFolder}"
     @click="toggle"
     @dblclick="changeType"
    >
        {{ model.name }}
        <span v-if="isFolder">[{{ open ? '-' : '+' }}]</span>
    </div>
    <!-- 这里是递归的地方 -->
    <ul v-show="open" v-if="isFolder">
        <item
         class="item"
         v-for="(model, index) in model.children"
         :key="index"
         :model="model"
        ></item>
        <li class="add" @click="addChild">+</li>
    </ul>
</li>
</template>

<script>
    var data = {
        name: 'My Tree',
        children: [
            {name: 'hello'},
            {name: 'world'},
            {
                name: 'others',
                children: [
                    {name: 'bye'},
                    {name: 'world'}
                ]
            }
        ]
    }
    export default {
        props: {
            model: Object
        },
        data() {
            return {
                open: false
            }
        },
        computed: {
            isFolder() {
                return this.model.children && this.model.children.length
            }
        },
        methods: {
            toggle() {
                if(this.isFolder) {
                    this.open = !this.open
                }
            },
            changeType() {
                if(!this.isFolder) {
                    this.$set(this.model, 'children', [{name:
                    'new stuff'}])
                    this.open = true
                }
            },
            addChild(e) {
                // 向当前数组中添加一项，而不是直接操作 DOM
                this.model.children.push({
                    name: 'new stuff'
                })
            }
        }
    }
</script>

<style>
    body {
        font-family: Menlo, Consolas, monospace;
        color: #444;
    }
    .item {
        cursor: pointer;
    }
    .bold {
        font-weight: bold;
    }
    ul {
        padding-left: 1em;
        line-height: 1.5em;
        list-style-type: dot;
    }
</style>
