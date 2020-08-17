  <template>
   <view class="map">
    <!-- <baidu-map
       class="map-contain"
       :scroll-wheel-zoom="true"
       :center="center"
       :zoom="zoom"
       style="height: 100vh"
       MapType="BMAP_SATELLITE_MAP"
       @ready="mapReady"
       :dragging="true"
     >
       <bm-marker
         @dragend="markerDrag"
         :position="{ lng: center.lng, lat: center.lat }"	
         :dragging="true"
         animation="BMAP_ANIMATION_BOUNCE"
         @click="infoWindowOpen"
       >
         <bm-info-window
           :show="show"
           @close="infoWindowClose"
           @open="infoWindowOpen"
         >
           <h6>经度：{{ center.lng }}</h6>
           <h6>纬度：{{ center.lat }}</h6>
 		  <h6>速度：{{ center.lat }}</h6>
 		  <h6>时间：{{ center.lat }}</h6>
         </bm-info-window>
       </bm-marker>
     </baidu-map> -->
   </view>
 </template>
 
 <script>
 //地图组件---按需引入
 import {
   BaiduMap,
   BmControl,
   BmView,
   BmAutoComplete,
   BmLocalSearch,
   BmMarker,
 } from "vue-baidu-map";
 export const PRODUCT_DEV = "http://122.191.199.47:8808";
 export default {
   data() {
     return {
       bm: null,
       show: false,
       zoom: 5, //缩放等级
       center: { lng: 0, lat: 0 },
     };
   },
   onLoad() {
   },
   mounted(){
       this.all()  
   },
   created() {},
   methods: {
        all(){
                     this.BaiduMap()
                     this.addMarker()
                 },
  BaiduMap(){

                /**地图初始化 */
                var map = new BMap.Map("all-map");    // 创建Map实例
                map.centerAndZoom(new BMap.Point(108.95,34.28), 5);  // 初始化地图,设置中心点坐标和地图级别

                // 添加地图类型控件

                map.addControl(new BMap.MapTypeControl({

                    mapTypes:[

                        BMAP_NORMAL_MAP,

                        BMAP_SATELLITE_MAP,

                        BMAP_HYBRID_MAP

                    ]})); 

                map.addControl(new BMap.NavigationControl());

                map.setCurrentCity("北京");          // 设置地图显示的城市 此项是必须设置的

                map.enableScrollWheelZoom(true);    //开启鼠标滚轮缩放

                window.map = map;//将map变量存储在全局

            },

            addMarker(){

                var markerArr=this.deviceArry

                map.centerAndZoom(new BMap.Point(markerArr[0].longitude,markerArr[0].latitude), 8);

                var point = new Array();//定义数组标注经纬信息

                var marker = new Array();//定义数组点对象信息

                var info = new Array();//定义悬浮提示信息
                for(var i = 0; i < markerArr.length; i++){//遍历

                    point[i] = new window.BMap.Point(markerArr[i].longitude,markerArr[i].latitude);

                    marker[i] = new window.BMap.Marker(point[i],{icon:myIcon});//把icon和坐标添加到Marker中

                    map.addOverlay(marker[i]);

                    var label = new window.BMap.Label(markerArr[i].name);

                    marker[i].setLabel(label);

                    info[i] = new window.BMap.InfoWindow(

                        "<div style='width:300px;'>"

                            +"<p>设备："+markerArr[i].name+"</p>"

                            +"<p>IMEI："+markerArr[i].terminalid+"</p>"

                            +"<p>速度："+markerArr[i].speed+"</p>"

                            +"<p>定位时间："+markerArr[i].locationdate+"</p>"

                            +"<p>通讯时间："+markerArr[i].insertdate+"</p>"

                            +"<p>总里程："+markerArr[i].mileage+"KM</p>"

                            +"<p>详细地址："+markerArr[i].address+"</p>"

                        +"</div>"

                    );//悬浮提示信息

                    this.addInfo(info[i],marker[i])

                }

            },

            addInfo(info,marker){

                marker.addEventListener("click", function(e){

                    var p = e.target

                    var point = new BMap.Point(p.getPosition().lng, p.getPosition().lat)

                    map.centerAndZoom(point, 14);

                    this.openInfoWindow(info)

                })

            },

},
}
 </script>
 
 <style></style>
 