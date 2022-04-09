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
    </l-map>
    <ul v-for="eachData in testData" :key="eachData.id">
      <li>{{ eachData.name }}</li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Watch, Vue } from "vue-property-decorator";
import { LMap, LTileLayer } from "vue2-leaflet";
import "leaflet/dist/leaflet.css";
import MyButton from "@/components/MyButton.vue";
import ResetButton from "@/components/ResetButton.vue";

type DataText = { [index: string]: string | number };
type DataList = DataText[] | [];

@Component({
  components: {
    MyButton,
    ResetButton,
    LMap,
    LTileLayer,
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
