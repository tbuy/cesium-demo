<template>
    <div id="app">
        <div id="cesiumContainer"></div>
    </div>
</template>

<script>
    import axios from 'axios'
    import Cesium from 'cesium/Cesium'
    import widgets from 'cesium/Widgets/widgets.css'

    export default {
        name: 'App',
        mounted() {
            var viewer = new Cesium.Viewer('cesiumContainer', {
                //动画部件
                animation: true,
                // 搜索控件
                geocoder: false,
                // 默认相机位置
                homeButton: false,
                // 图层选择控件
                baseLayerPicker: false,
                // 阴影
                shadows: false,
                // 数据版权属性
                creditDisplay: false,
                // 如果设为true，将在一个HTML面板中显示错误信息
                showRenderLoopErrors: false,
                // 时间轴
                timeline: true,
                //信息盒子
                infoBox: false,
                // 指示器
                selectionIndicator: false,
                // 全屏切换
                fullscreenButton: false,
                // 帮助控件
                navigationHelpButton: false,
                // 2d, 3d模式切换
                sceneModePicker: false,
                // 只选择2d
                sceneMode: Cesium.SceneMode.SCENE2D,
                // 全屏时渲染的HTML元素
                fullscreenElement: document.body,
            });

        }
    }

</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

</style>
