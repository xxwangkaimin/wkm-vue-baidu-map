<template lang="md">

# 地址解析

地址解析服务提供从地址转换到经纬度的服务，反之，逆地址解析则提供从经纬度坐标转换到地址的转换功能。。

## 提供的转换类

Geocoder：逆/地址解析，用于坐标与地址间的相互转换。详情见类参考

## 逆地址解析服务

根据坐标点获得该地点的地址描述，是地址解析的逆向转换。 您可以通过Geocoder.getLocation()方法获得地址描述。当解析工作完成后，您提供的回调函数将会被触发。如果解析成功，则回调函数的参数为GeocoderResult对象，否则为null。 在构造Geocoder对象时，可以增加参数{extensions_town: true}来获得乡镇级数据，仅限国内。

#### 代码

```html

<baidu-map class="bd-map" @ready="MpReady" :center="{lng: lng, lat: lat}" :zoom="15" ak="v3Xv9NKiHhd6SlezuSnSvSnYZWrYVpKo">
</baidu-map>
<script>
    import BaiduMap from 'vue-baidu-map/components/map/Map'

    export default {
        name: "electronicFence",
        components: {
            BaiduMap
        },
        data() {
            return {
                lng: '113.81455321958914',
                lat: '34.79784536264466',
                currentAddr: '',
            }
        },
        methods: {
            /**
             * 地图准备完毕
             * @param BMap
             * @param map
             * @constructor
             */
            MpReady({BMap, map}) {
                this.getBdMapLocation(BMap, map)
            },
            /**
             * 根据经纬度获取位置信息
             * @param lng
             * @param lat
             * @param BMap
             * @param map
             */
            getBdMapLocation(BMap, map) {
                const myGeo = new BMap.Geocoder({extensions_town: true});
                let _this = this
                myGeo.getLocation(new BMap.Point(_this.lng, _this.lat), function (res) {
                    let {surroundingPois} = res
                    if (surroundingPois instanceof Array && surroundingPois.length > 0) {
                        let item = surroundingPois[0]
                        _this.currentAddr = (item.address || '') + (item.title || '')
                    } else {
                        let {addressComponents} = res
                        _this.currentAddr = addressComponents.province + ", " + addressComponents.city + ", " + addressComponents.district + ", " + addressComponents.town + ", " + addressComponents.street
                    }
                });
            }
        }
    }
</script>
```

#### 预览

<doc-preview>
<div>
<baidu-map @ready="MpReady" :center="{lng: lng, lat: lat}" version="3" :zoom="15">
<bm-view class="map"></bm-view>
<bm-marker v-if="lat && lng" :position="{lng: lng, lat: lat}"></bm-marker>
</baidu-map>
<div style="padding: 10px">当前位置：{{currentAddr}}</div>
</div>
</doc-preview>
</template>

<script>
export default {
  data () {
    return {
        lng: '113.81455321958914',
        lat: '34.79784536264466',
        currentAddr: ''
    }
  },
methods: {
    /**
     * 地图准备完毕
     * @param BMap
     * @param map
     * @constructor
     */
    MpReady({BMap, map}) {
      this.getBdMapLocation(BMap, map)
    },
 /**
     * 根据经纬度获取位置信息
     * @param lng
     * @param lat
     * @param BMap
     * @param map
     */
    getBdMapLocation(BMap, map) {
      const myGeo = new BMap.Geocoder({extensions_town: true});
      let _this = this;
      myGeo.getLocation(new BMap.Point(_this.lng, _this.lat), function (res) {
        let {surroundingPois} = res;
        if (surroundingPois instanceof Array && surroundingPois.length > 0) {
          let item = surroundingPois[0];
          _this.currentAddr = (item.address || '') + (item.title || '')
        } else {
          let {addressComponents} = res;
          _this.currentAddr = addressComponents.province + ", " + addressComponents.city + ", " + addressComponents.district + ", " + addressComponents.town + ", " + addressComponents.street
        }
      });
    },
}
}
</script>
