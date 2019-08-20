<template>
    <div id="app">
        <div id="cesiumContainer"></div>
    </div>
</template>

<script>
    import axios from 'axios'
    import Cesium from 'cesium/Cesium'
    import widgets from 'cesium/Widgets/widgets.css'
    import airline from '../static/json/airline.json'

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

            //场景设置（设置背景色，隐藏地图）
            viewer.scene.backgroundColor = new Cesium.Color.fromCssColorString('#0B1629')
            viewer.scene.globe.show = !viewer.scene.globe.show;

            //画地图
            viewer.dataSources.add(Cesium.GeoJsonDataSource.load('../static/json/world.json', {
                stroke: new Cesium.Color.fromCssColorString('pink'),
                fill: new Cesium.Color.fromCssColorString('pink').withAlpha(0.9),
                strokeWidth: 3,
                markerSymbol: '?'
            }));

            //初始化动画
            this.setView(viewer, 160.08, 8, 40000000);
            this.addCityPoint(viewer, airline);
        },
        methods: {
            /**
             * 设置相机位置
             * viewer 视图对象
             * longitude 经度
             * dimension 纬度
             * height 高度
             */
            setView: function (viewer, longitude, dimension, height) {
                //设置相机位置
                viewer.camera.setView({
                    destination: Cesium.Cartesian3.fromDegrees(longitude, dimension, height),
                    orientation: {
                        heading: Cesium.Math.toRadians(0),
                        pitch: Cesium.Math.toRadians(-90),
                        roll: Cesium.Math.toRadians(0)
                    }
                });
            },
            /**
             * 添加圆点和文字
             * viewer 视图对象
             * cityJson 城市json
             */
            addCityPoint: function (viewer, cityJson) {
                viewer.scene.postProcessStages.fxaa.enabled = false;
                // 添加圆点和文字
                cityJson.forEach(item => {
                    viewer.entities.add({
                        // id: item.id,
                        name: item[item.length - 1].name + "dian",

                        position: Cesium.Cartesian3.fromDegrees(item[item.length - 1].longitude,
                            item[item.length - 1].dimension, 10000000),
                        //点样式
                        point: {
                            pixelSize: 4,
                            color: Cesium.Color.fromCssColorString('#0B93F6'),
                            outlineColor: Cesium.Color.WHITE,
                            outlineWidth: 1
                        },
                        //字体标签样式
                        label: {
                            text: "",
                            font: 'normal 32px MicroSoft YaHei',
                            scale: 0.5, //这里非常巧妙的先将字体大小放大一倍在缩小一倍
                            color: Cesium.Color.RED,
                            style: Cesium.LabelStyle.FILL_AND_OUTLINE,
                            outlineWidth: 1,
                            //垂直位置
                            // verticalOrigin : Cesium.VerticalOrigin.BUTTON,
                            //中心位置
                            pixelOffset: new Cesium.Cartesian2(0, 20)
                        },

                    });
                });
            },
        },
    }

</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
    }

    .cesium-viewer-bottom,
    .cesium-viewer-animationContainer,
    .cesium-viewer-timelineContainer {
        display: none !important;
    }

</style>
