<template>
    <div>
        <!-- polygraph -->
        <g>
            <polygon :points="ponits"></polygon>
            <circle cx="100" cy="100" r="80"></circle>
            <axis-label
             v-for="(stat, index) in stats"
             :stat="stat"
             :key="index"
             :index="index"
             :total="stats.length"
            >

            </axis-label>
        </g>
        <!-- axis label -->
        <text :x="point.x" :y="point.y">{{ stat.label }}</text>
        <!-- root -->
        <div id="demo">
            <svg width="200" height="200">
                <polygon :stats="stats"></polygon>
            </svg>

            <div v-for="(stat,index) in stats" :key="index">
                <label>{{ stat.label }}</label>
                <input type="range" :value="stat.value">
                <span>{{ stat.value }}</span>
                <button @click="delStat(stat)">X</button>
            </div>

            <form>
                <input name="newStat" type="text" v-model="newStat">
                <button @click="addStat">Add a stat</button>
            </form>

            <p>* input[type="range"] requires IE10 or above.</p>
        </div>
    </div>
</template>

<script>
    export var obj1 = {
        props: ['stats'],
        computed: {
            ponits() {
                var total = this.stats.length
                return this.stats.map(function(stat, index) {
                    var point = valueToPoint(stat.value, index, length)
                    return point.x + ',' + point.y
                }).join(' ')
            }
        }
    }

    function valueToPoint(value, index, total) {
        var x = 0
        var y = -value * 0.8
        var angle = Math.PI * 2 / total * index
        var cos = Math.cos(angle)
        var sin = Math.sin(angle)
        var tx = x * cos - y * sin + 100
        var ty = x * sin + y * cos + 100
        return {
            x: tx,
            y: ty
        }
    }

    export var obj2 = {
        props: {
            stat: Object,
            index: Number,
            total: Number
        },
        computed: {
            point() {
                // 产生一个文本的坐标
                return valueToPoint(
                    +this.stat.value + 10,
                    this.index,
                    this.total
                )
            }
        }
    }

var stats = [
  {
    "label": "A",
    "value": 100
  },
  {
    "label": "B",
    "value": 100
  },
  {
    "label": "C",
    "value": 100
  },
  {
    "label": "D",
    "value": 100
  },
  {
    "label": "E",
    "value": 100
  },
  {
    "label": "F",
    "value": 100
  }
]
    export var obj3 = {
        data() {
            return {
                stats,
                newStat: ''
            }
        },
        methods:{
            delStat(stat) {
                if(this.stats.length > 3) {
                    this.stats.splice(this.stats.indexOf(stat), 1)
                }else {
                    alert('Can\'t delete more!')
                }
            },
            addStat(e) {
                e.preventDefault();
                if (!this.newStat) return;
                var _this = this
                this.stats.push({
                    "label": _this.newStat,
                    "value": 100
                })
                this.newStat = ''
            }
        }
    }
</script>

<style>
body {
    font-family: 'Helvetica Neue', Arial, sans-serif;
    font-size: 14px;
    color: #333;
}

polygon {
    fill: #42b983;
    opacity: 0.75;
}

circle {
    fill: transparent;
    stroke: #999;
}

text {
    font-family: 'Helvetica Neue', Arial, sans-serif;
    font-size: 10px;
    fill: #666;
}

label {
    display: inline-block;
    margin-left: 10px;
    width: 20px;
}
#raw {
    position: absolute;
    top: 0;
    left: 300px;
}
</style>