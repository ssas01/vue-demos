<template>
    <div id="demo">
        <h1>Latest Vue.js Commits</h1>
        <template v-for="branch in branches">
            <input
             :id="branch"
             :value="branch"
             :key="branch"
             name="branch"
             v-model="currentBranch"
            />
            <label
             :for="branch"
             :key="branch"
            >{{ branch }}</label>
        </template>
        <p>vuejs/vue@{{ currentBranch }}</p>
        <ul>
            <li
             v-for="record in commits"
             :key="record"
            >
                <a
                 :href="record.html_url"
                 target="_blank"
                 class="commit"
                    {{ record.sha.slice(0,7) }}
                ></a>
                -
                <span class="message">
                    {{ record.commit.message | truncate }}
                </span>
                <br/>
                by
                <span class="author">
                    <a 
                     :href="record.author.html_url"
                     target="_blank"
                    >
                        {{ record.commit.author.name }}
                    </a>
                </span>
                at
                <span class="date">
                    {{ record.commit.author.date | formatDate }}
                </span>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                branches: ['master', 'div'],
                currentBranch: 'master',
                commits: null
            }
        },
        created() {
            this.fetchData()
        },
        watch: {
            'currentBranch': fetchData
        },
        // 感觉过滤器的使用和监视数据的使用长知识了
        filters: {
            truncate: function (v) {
                var newline = v.indexOf('\n')
                return newline > 0  ? v.slice(0, newline) : v
            },
            formatDate: function(v) {
                return v.replace(/T|Z/g, ' ')
            }
        },
        methods: {
           fetchData() {
               var xhr = new XMLHttpRequest()
               var _this = this
               xhr.open('GET', apiURL + _this.currentBranch)
               xhr.onload = function() {
                   _this.commits = JSON.parse(xhr.responseText)
               }
               xhr.send()
               
           } 
        }
    }
</script>

<style>
    #demo {
        font-family: 'Helvetica', Arial, sans-serif;
    }
    a {
        text-decoration: none;
    }
    li {
        line-height: 1.5em;
        margin-bottom: 20px;
    }
    .author, .date {
        font-weight: bold;
    }
</style>