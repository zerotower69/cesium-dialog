<script setup lang="tsx">
import {onMounted, ref} from "vue";
import * as Cesium from "cesium";
import {clickInit} from "./map/clickPoint"
import {createPopup} from "@/components/dialog";
import Content from "@/components/Content.vue"

Cesium.Ion.defaultAccessToken="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI3Y2YzMzhjNy03MmViLTRlNjItOWMzMi1mYzg3MmZkMGE5ZjYiLCJpZCI6MTIwNzI1LCJpYXQiOjE2NzMyNDczMDF9.FVuv-GXxuLCZvztzncNHaHhKyqYLQKaS1GO7v1MvtGU"


let viewer:Cesium.Viewer|null =null;
const containerRef =ref();

//
function loadIcon(){
    const entity = (viewer as Cesium.Viewer).entities.add({
        name:"test",
        position:Cesium.Cartesian3.fromDegrees(
            118.30594,  32.39439,0
        ),
        cylinder:{
            topRadius:500,
            bottomRadius:500,
            length:200,
            material:Cesium.Color.BLUE
        }
    })
}

onMounted(()=>{
    let viewerOption = {
        baseLayerPicker: false, // 地图切换控件(底图以及地形图)是否显示,默认显示true  （图中6）
        fullscreenButton: false, // 全屏按钮,默认显示true  （图中4）
        geocoder: false, // 地名查找,默认true  （图中9）
        homeButton: false, // 主页按钮，默认true  （图中8）
        infoBox: false, // 点击要素之后显示的信息,默认true
        selectionIndicator: false, // 选中元素显示,默认true
        CreditsDisplay: false, // 展示数据版权属性
        shouldAnimate: true,
        sceneModePicker: false, // 二三维切换按钮
        navigationHelpButton: false, // 问号图标，导航帮助按钮，显示默认的地图控制帮助
        contextOptions: {
            webgl: {
                preserveDrawingBuffer: true, // cesium状态下允许canvas转图片convertToImage
            },
        },
    };
    //初始化地球
    viewer= new Cesium.Viewer(containerRef.value,{
        ...viewerOption,
        terrainProvider: Cesium.createWorldTerrain(),
        //高德影像地形地图
        imageryProvider: new Cesium.UrlTemplateImageryProvider({
            url:"https://webst02.is.autonavi.com/appmaptile?style=6&x={x}&y={y}&z={z}"
        })
    });
    // 高德影像标记地图
    viewer.imageryLayers.addImageryProvider(new Cesium.UrlTemplateImageryProvider({
        url:"http://webst02.is.autonavi.com/appmaptile?x={x}&y={y}&z={z}&lang=zh_cn&size=1&scale=1&style=8"
    }))
    loadIcon();
    clickInit(viewer as Cesium.Viewer,(entity,coord)=>{
        console.log(entity,coord)
        //
       if(entity.id){
           createPopup(
               viewer as Cesium.Viewer,
               coord,
               {top:0,left:135},
               {
                   default:()=>(<Content/>)
               }
           )
       }
    });
    (viewer as Cesium.Viewer).camera.flyTo({
        destination:Cesium.Cartesian3.fromDegrees(
            118.345332,
        32.18228,
        6721.35,
        ),
        orientation:{
            heading: Cesium.Math.toRadians(344.58),
            pitch: Cesium.Math.toRadians(-27.44),
            roll: 0,
        }
    })
})
</script>

<template>
  <div class="cesium-app">
      <div class="cesium-container" ref="containerRef"></div>
  </div>
</template>

<style scoped>
.cesium-app{
    height: 100%;
    width: 100%;
}
.container{
    height: 100%;
    width: 100%;
}
</style>
