<template style="height: 100%; margin: 0">
    <div id="container" style="height: 100%"></div>
    </template>


<script type="text/javascript" src="https://registry.npmmirror.com/echarts/5.5.1/files/dist/echarts.min.js"></script>
<script type="text/javascript">
    var dom = document.getElementById('container');
    var myChart = echarts.init(dom, null, {
        renderer: 'canvas',
        useDirtyRect: false
    });
    var app = {};

    var option;

    option = {
        tooltip: {
            trigger: 'item'
        },
        legend: {
            top: '5%',
            left: 'center'
        },
        series: [{
            name: 'Access From',
            type: 'pie',
            radius: ['40%', '70%'],
            avoidLabelOverlap: false,
            padAngle: 5,
            itemStyle: {
                borderRadius: 10
            },
            label: {
                show: false,
                position: 'center'
            },
            emphasis: {
                label: {
                    show: true,
                    fontSize: 40,
                    fontWeight: 'bold'
                }
            },
            labelLine: {
                show: false
            },
            data: [{
                value: 1048,
                name: 'Search Engine'
            }, {
                value: 735,
                name: 'Direct'
            }, {
                value: 580,
                name: 'Email'
            }, {
                value: 484,
                name: 'Union Ads'
            }, {
                value: 300,
                name: 'Video Ads'
            }]
        }]
    };

    if (option && typeof option === 'object') {
        myChart.setOption(option);
    }

    window.addEventListener('resize', myChart.resize);
</script>

</html>