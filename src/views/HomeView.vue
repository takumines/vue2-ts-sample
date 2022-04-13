<template>
  <div class="home">
    <p>{{ greetText }}</p>
    <p>挨拶した回数：{{ count }}回</p>
    <p v-if="isRegulars">いつもありがとうございます</p>
    <p>
      <MyButton :greet="greetText" @click="onMyButtonClicked"></MyButton>
    </p>
    <p>
      <ResetButton v-model="greetText"></ResetButton>
    </p>
    <p>
      <v-btn @click="addTestDataFunc">push</v-btn>
    </p>
    <l-map
      ref="map"
      style="height: 600px"
      :zoom="map.zoom"
      :center="map.center"
    >
      <l-tile-layer :url="map.url"></l-tile-layer>
      <template>
        <l-marker
          v-for="(marker, index) in map.markerPlaces"
          :key="marker.id + index"
          :lat-lng="marker.latlon"
        >
          <l-popup>
            <div class="primary--text">{{ marker.name }}</div>
            <div v-for="(title, index) in marker.titles" :key="'tit-' + index">
              <a :href="title.uri" target="_blank">{{ title.title }}</a>
            </div>
          </l-popup>
        </l-marker>
      </template>
    </l-map>
    <ul v-for="eachData in testData" :key="eachData.id">
      <li>{{ eachData.name }}</li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Watch, Vue } from "vue-property-decorator";
import { LMap, LTileLayer, LMarker, LPopup } from "vue2-leaflet";
import { latLng } from "leaflet";
import { LatLng } from "leaflet";
import "leaflet/dist/leaflet.css";
import MyButton from "@/components/MyButton.vue";
import ResetButton from "@/components/ResetButton.vue";

type DataText = { [index: string]: string | number };
type DataList = DataText[] | [];
type MarkerPlace = {
  name: string;
  latlon: LatLng;
  titles: {
    title: string;
    uri: string;
  }[];
  id: number;
};
type MarkerPlaces = MarkerPlace[];

@Component({
  components: {
    MyButton,
    ResetButton,
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
  },
})
export default class HomeView extends Vue {
  private count = 0;
  public greetText = "Hello";
  public testData: DataList = [];
  public map = {
    url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
    zoom: 8,
    center: [34, 137],
    markerPlaces: [
      {
        name: "宮崎県",
        latlon: latLng(35, 135),
        titles: [{ title: "マンゴーシティ", uri: "https://www.google.com/" }],
        id: 1,
      },
      {
        name: "兵庫県",
        latlon: latLng(35, 136),
        titles: [{ title: "なんとか市", uri: "https://www.google.com/" }],
        id: 2,
      },
    ] as MarkerPlaces,
  };

  public get isRegulars(): boolean {
    return this.count >= 5;
  }

  @Watch("count")
  public countChanged() {
    if (this.count === 5) {
      alert("常連になりました");
    }
  }

  public onMyButtonClicked(count: number) {
    this.count = count;
    this.greetText = "こんにちは";
  }

  public addTestDataFunc() {
    if (this.testData.length === 0) {
      let dataList = [];
      let dataText: DataText;
      for (let n = 0; n < 5; n++) {
        dataText = {
          id: n,
          name: "name" + n,
        };
        dataList.push(dataText);
        dataText = {};
      }
      this.testData = dataList;
    } else {
      this.testData = [];
    }
  }
}
</script>
