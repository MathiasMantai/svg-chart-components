<script>
    import RingChart from './RingChart.jsx';

    export default {
        props: {
            radius: {
                type: Number,
                default: 100
            }
        },
        methods: {
            arcEndPoint(angle, radius, totalSize) {
                return  [
                    Math.cos(angle * Math.PI / 180) * radius + totalSize / 2,
                    Math.sin(angle * Math.PI / 180) * -radius + totalSize / 2
                ]
            }
        }
    }
</script>

<template>
    <div>
        {{arcEndPoint(45, radius, 300)}}
    </div>
</template>