<script>
    export default {
        data() {
            return {
                totalData: 0,
                svgRingParts: []
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
            this.data.map( (dataElement) => {
                this.svgRingParts.push(this.dataToAngle(dataElement, this.totalData));
            });

            //create ring parts command

        }
    }
</script>

<template>
    {{ svgRingParts }}
</template>