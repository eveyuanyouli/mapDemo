<template>
	<view class="map">
		<baidu-map class="map-contain" :scroll-wheel-zoom="true" :center="center" :zoom="zoom" style="height: 100vh" MapType="BMAP_SATELLITE_MAP"
		 @ready="mapReady" :dragging="true">
			<bm-marker v-for="(marker,index) in markers" :key="index" @dragend="markerDrag" :position="{ lng: marker.lng, lat: marker.lat }"
			 :dragging="true" animation="BMAP_ANIMATION_BOUNCE" @click="infoWindowOpen(marker)">
				<bm-info-window :show="marker.showFlag" @close="infoWindowClose(marker)" @open="infoWindowOpen">
					<h6>经度：{{ marker.lng }}</h6>
					<h6>纬度：{{ marker.lat }}</h6>
					<h6>速度：{{ center.lng }}</h6>
					<h6>时间：{{ center.lat }}</h6>
				</bm-info-window>
			</bm-marker>
		</baidu-map>
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
		BmInfoWindow
	} from "vue-baidu-map";

	export const PRODUCT_DEV = "http://122.191.199.47:8808";

	export default {
		components: {
			BaiduMap,
			BmControl,
			BmView,
			BmAutoComplete,
			BmLocalSearch,
			BmMarker,
			BmInfoWindow
		},
		data() {
			return {
				bm: null,
				show: false,
				zoom: 10, //缩放等级
				center: {
					lng: 116.405,
					lat: 39.901,
				},
				markers: [{
						lng: 120.404,
						lat: 49.900,
						showFlag: true
					},
					{
						lng: 104.73,
						lat: 31.47,
						showFlag: true //flag放在每一条数据里
					},
				]
			};
		},
		onLoad() {},
		created() {},
		methods: {
			infoWindowClose(marker) {
				marker.showFlag = false
			},
			infoWindowOpen(marker) {
				marker.showFlag = true
			},

			markerDrag(x) {},
			getData() {
				let that = this;
				var geolocation = new BMap.Geolocation();

				uni.request({
					url: PRODUCT_DEV + "/path/getPathByTerminalIds",
					method: "POST",
					success: (res) => {
						console.log("数据=========>" + JSON.stringify(res.data));
						that.show = true;
						
						/*    var pointArr = [];
						    let point = new BMap.Point(res.data.lng, res.data.lat);
						    pointArr.push(point);
						    //百度api
						    new BMap.Convertor().translate(pointArr, 1, 5, (res1) => {
						      if (res1.status == 0) {
						        that.center.lng = res1.points[0].lng;
						        that.center.lat = res1.points[0].lat;
						      }
						    }); */
						console.log(res.data.data);

					},
					fail: (res) => {
						console.log("fail:====" + res);
					},
				});
				setTimeout(() => {
					that.getData();
				}, 30000);
			},

			// 地图初始化
			mapReady({
				BMap,
				map
			}) {

				this.getData();
			},
			// 定位成功之后触发事件
			getMyLocation() {},
			infoWindowClose() {
				this.show = false;
			},
			infoWindowOpen() {
				this.show = true;
			},
		},
	};
</script>

<style></style>
