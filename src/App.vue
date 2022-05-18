<template>
  <div id="app">
    <h1 class="header">Welcome to Opportunity</h1>
    <h2 class="headerdate">{{ currentDate() }}</h2>
    <!--<p id="noEntries" v-if="!value">No appointments today!</p>  else is also missing to finish this-->

    <ul>
      <li class="unlist bluebox" v-for="event in entries" :key="event.id">
        <span class="hour">
          {{ event[0] }} - {{ event[1].replaceAll("/", " .") }}
        </span>
        <h3 class="activity">{{ event[2] }}</h3>
        <span class="who"> {{ event[3] }} </span>
      </li>
    </ul>

    <footer>
      <div class="footer">
        <img class="oppologos" alt="Oppo logo" src="./assets/opportunity.png" />
        <img class="oppologos" alt="Sag logo" src="./assets/sag.png" />
        <img class="oppologos" alt="SZ logo" src="./assets/stadt_zurich.png" />
      </div>
    </footer>
  </div>
</template>

<script>
import axios from "axios"; // axios is a library for making HTTP requests to the backend

export default {
  name: "App",
  data() {
    return {
      currenthour: "10.00 Uhr",
      firstline: "Title",
      secondline: "Description",
      title: "Welcome to Opportunity",
      sheet_id: "1a81aI0Y8ViZO0tI92h2YSMqVQJ8hmNNMyMylXgvwiU4",
      api_token: "AIzaSyA-qeDXOhEeQDA0vQf7LgkF7DQtGnAtmAU",
      entries: [],
    };
  },
  computed: {
    // computed properties are like data properties, but with a method combined and it gets executed automatically, instead of calling a function explicitly
    gsheet_url() {
      return `https://sheets.googleapis.com/v4/spreadsheets/${this.sheet_id}/values:batchGet?ranges=A2%3AE100&valueRenderOption=FORMATTED_VALUE&key=${this.api_token}`;
    },
  },

  methods: {
    getData() {
      axios.get(this.gsheet_url).then((response) => {
        this.entries = response.data.valueRanges[0].values;
      });
    },

    currentDate() {
      const current = new Date();
      const day = current.getDate();
      const month = current.getMonth() + 1;
      const year = current.getFullYear();
      const dateTime = day + "." + month + "." + year;
      if (month < 10) {
        return day + "." + "0" + month + "." + year;
      }
      return dateTime;
    },
  },

  mounted() {
    this.getData();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@500;900&display=swap");

body {
  background: #e5e5e5;
}

#app {
  font-family: "Inter";
  font-style: normal;
}

.unlist {
  list-style-type: none;
}

.header {
  font-weight: 900;
  font-size: 62px;
  line-height: 75px;
  color: #323d4a;
  padding-left: 80px;
  padding-top: 60px;
}
.headerdate {
  font-weight: 500;
  font-size: 62px;
  line-height: 75px;
  color: #9aa7b1;
  padding-left: 80px;
}

.bluebox {
  background: #0f05a0;
  margin: 40px;
  padding-left: 60px;
  padding-top: 60px;
  padding-bottom: 60px;
}

.hour {
  font-weight: 900;
  font-size: 28px;
  line-height: 36px;
  color: #eb5e00;
}

.activity {
  font-weight: 900;
  font-size: 28px;
  line-height: 36px;

  color: #ffbfab;
}

.who {
  font-weight: 500;
  font-size: 28px;
  line-height: 36px;

  color: #ffbfab;
}

.oppologos {
  height: 50px;
}

.footer {
  display: flex;
  justify-content: space-between;
  box-sizing: border-box;
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  padding: 40px;
  background: #fff;
}
</style>
