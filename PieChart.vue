<script>
    export default {
        data() {
            return {
                totalData: 0,
                svgPieParts: [],
                viewbox: ""
            }
        },
        props: {
            radius: {
                type: Number,
                default: 100
            },
            data: {
                type: Array,
                default: [10,10,10,10,10]
            },
            color: {
                type: Array,
                default: ["#ff6200", "green", "red", "blue", "#fff"]
            }

        },
        methods: {
            arcEndPoint(angle, radius, totalSize) {
                return  [
                    Math.cos(angle * Math.PI / 180) * radius + totalSize / 2,
                    Math.sin(angle * Math.PI / 180) * -radius + totalSize / 2
                ]
            },
            calcTotalData(data) {
                let tmp = 0;
                data.map(dataElement => tmp += dataElement);
                return tmp;
            },
            dataToAngle(dataElement, totalData) {
                let dataElementPercentage = (dataElement * 100) / totalData;
                let angle = (dataElementPercentage * 360) / 100;
                console.log(dataElement, totalData);
                return angle;
            },
            outputChart() {
                return h('div', {id: 'test'}, []);
            }
        },
        mounted() {
            //calculate sum of data
            this.totalData = this.calcTotalData(this.data);
            
            //calculate angles based on percentage of 360 degrees
            let startX = this.radius*2;
            let startY = this.radius;
            let angleSum = 0;
            let cnt = 0;
            this.data.map( (dataElement) => {
                angleSum += this.dataToAngle(dataElement, this.totalData);
                let currentEndPoint = this.arcEndPoint(angleSum, this.radius, this.radius * 2);
                this.svgPieParts.push({path: "M " + startX + " " + startY + " " + "A " + this.radius + " " + this.radius + " 0 0 0 " + currentEndPoint[0] + " " + currentEndPoint[1] + " L " + this.radius + " " + this.radius, color: this.color[cnt]});

                startX = currentEndPoint[0];
                startY = currentEndPoint[1];
                cnt++;
            });

            this.viewbox = "0 0 " + this.radius*2 + " " + this.radius*2;



        }
    }
</script>

<template>
    <svg :viewbox="viewbox" :width="(radius * 2)" :height="(radius * 2)">
        <path v-for="path in svgPieParts" :d="path.path" :fill="path.color" />
    </svg>
</template>