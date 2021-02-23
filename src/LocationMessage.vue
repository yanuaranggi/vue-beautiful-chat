<template>
  <div class="sc-message--text" :style="messageColors" v-loading="data.proccessUpload ? data.proccessUpload : false">
    <l-map
      :center="center"
      :zoom="currentZoom"
      @update:zoom="zoomUpdate"
      :options="mapOptions"
      @update:center="centerUpdate"
      style="height: 300px">
      <l-tile-layer
        :url="url"
        :attribution="attribution"
      />
      <l-marker :lat-lng="center"/>
    </l-map>
    <p v-if="data.meta" class='sc-message--meta' :style="{color: messageColors.color}">{{data.meta}}</p>
  </div>
</template>

<script>
import escapeGoat from 'escape-goat'
import Autolinker from 'autolinker'
import * as EmailValidator from 'email-validator';
import * as Vue2Leaflet from 'vue2-leaflet';
import { latLng, Icon } from "leaflet";
delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
});
import { LMap, LTileLayer, LMarker, LPopup, LTooltip } from "vue2-leaflet";
import 'leaflet/dist/leaflet.css';

const fmt = require('msgdown')

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker,
  },
  props: {
    data: {
      type: Object,
      required: true
    },
    messageColors: {
      type: Object,
      required: true
    },
    messageStyling: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      location: {
        lat: 0,
        lng: 0
      },
      currentZoom: 18,
      center: latLng(),
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      mapOptions: {
        zoomSnap: 0.5
      },
    }
  },
  computed: {
    messageText() {
      return this.data.text
    }
  },
  methods: {
    zoomUpdate(zoom) {
      this.currentZoom = zoom;
    },
    centerUpdate(center) {
      this.center = center;
    },
  },
  mounted() {
    console.log("Type location", this.data)
    var [ lat, lng ] = this.data.text.split(",").map(v => v.trim())
    this.location = {
      lat: parseFloat(lat),
      lng: parseFloat(lng)
    }
    this.center = latLng(lat, lng)
    console.log(this.location)
  }
}
</script>

<style scoped>
a.chatLink {
  color: inherit !important;
}
</style>
