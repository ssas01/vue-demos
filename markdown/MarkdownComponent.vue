<template>
    <div id="editor">
        <textarea
         :value="input"
         @input="update"
        ></textarea>
        <div v-html="compiledMarkdown"></div>
    </div>
</template>

<script>
    // 将 v-model 拆开
    // compiledMarkdown 计算属性
    export default {
        data() {
            return {
                input: '#hello'
            }
        },
        computed: {
            compiledMarkdown() {
                return marked(this.input, {sanitize: true})
            }
        },
        methods: {
            update: _.debounce(function() {
                this.input = e.target.value
            }, 300)
        }
    }
</script>

<style>
    html, body, #editor {
        margin: 0;
        height: 100%;
        font-family: 'Helvetica Neue', Arial, sans-serif;
        color: #333;
    }

    textarea, #editor div {
        display: inline-block;
        width: 49%;
        height: 100%;
        vertical-align: top;
        box-sizing: border-box;
        padding: 0 20px;
    }
    
    code {
        color: #666;
    }
</style>