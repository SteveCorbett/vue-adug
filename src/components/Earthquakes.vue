<template>
  <v-container>
    <v-layout column align>
      <v-row xs12>
        <v-col cols="12">
          <div class="text-h4">{{title}}</div>
        </v-col>
        <v-flex xs12 sm6 md5 lg3>
          <v-text-field v-model="filter" label="Filter" @input="filterResults" clearable></v-text-field>
        </v-flex>

        <v-list subheader three-line xs9 sm6 md4 lg3>
          <div v-for="feature in features" :key="feature.id">
            <v-list-item v-if="feature.visible">
              <v-list-item-content>
                <v-list-item-title>
                  <a :href="feature.url" target="_blank" rel="noopener noreferrer">{{feature.title}}</a>
                </v-list-item-title>
                <v-list-item-subtitle>
                  Magnitude: {{feature.mag}}
                  <br />
                  Time: {{feature.time}}
                </v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </div>
        </v-list>
      </v-row>
      {{info}}
    </v-layout>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  mounted() {
    axios
      .get(
        "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/2.5_day.geojson"
      )
      .then(response => {
        this.info = response;
        this.title = response.data.metadata.title;
        response.data.features.forEach(feature => {
          this.features.push({
            id: feature.properties.id,
            title: feature.properties.title,
            mag: feature.properties.mag,
            time: new Date(feature.properties.time).toISOString(),
            url: feature.properties.url,
            visible: true
          });
        });
      })
      .catch(error => console.log(error));
  },
  methods: {
    filterResults() {
      this.features.forEach(feature => {
        if (this.filter == null) {
          feature.visible = true;
        } else {
          const title = feature.title;
          feature.visible = title
            .toLowerCase()
            .includes(this.filter.toLowerCase());
        }
      });
    }
  },
  data() {
    return {
      title: "Earthquakes!",
      features: [],
      info: null,
      filter: null
    };
  }
};
</script>
