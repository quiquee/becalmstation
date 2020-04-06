<template>
  <div class="sensor">
    {{ type }}

    <ul>
      <li>Status: {{ status }}</li>
      <li>UpStatus: {{ upstatus }}</li>
      <li>Unit: {{ unit }}</li>
      <li>Last Measure: {{ lastMeasure }}</li>
      <li>Highest: {{ highMeasure }}</li>
      <li>Lowest: {{ lowMeasure }}</li>
      <li>HighAlert: {{ MaxAlertVal }}</li>
      <li>LowAlert: {{ MinAlertVal }}</li>
    </ul>
  </div>
</template>

<script>
const serverPort = "http://192.168.0.22:8888/";
const refreshPeriod = 2000;
import axios from "axios";
export default {
  name: "Sensor",
  props: {
    type: String,
    endpoint: String,
    port: String,
    lowErrMeasure: String,
    highErrMeasure: String ,
  },
  data: () => {
    return {
      status: "",
      upstatus: "",
      lastMeasure: -1,
      highMeasure: -1,
      lowMeasure: -1,
      unit: -1,
      MaxHighVal: -1,
      MinLowVal: -1,
      MaxAlertVal: -1,
      MinAlertVal: -1

    }
  },
  mounted() {

      this.updateStatus();

  },
  methods: {
    updateStatus: function () {
      var ts = new Date();
      axios
        .get(serverPort + this.endpoint)
        .then( res => {
          if (res.data > this.highErrMeasure ||
              res.data < this.lowErrMeasure ) {
                  this.status="ERROR Medida incorrecta: " + res.data  ;
              }
          else
              {
                var payload = { measure_type: this.type,
                            measure_value: res.data,
                            date_generation: ts.toJSON() } ;
                axios
                .put("http://becalm.ngrok.io/data-sensor/2?id_device=1", payload)
                .then(this.upstatus='OK')
                .catch(error => this.upstatus=error );
                this.status="OK"  ;
                this.lastMeasure = res.data;
                if (this.highMeasure==-1){ this.highMeasure=res.data };
                if (this.lowMeasure==-1){ this.lowMeasure=res.data };
                if (res.data > this.highMeasure){ this.highMeasure=res.data };
                if (res.data < this.lowMeasure){ this.lowMeasure=res.data };
              }
        }).catch(error => this.status=error) ;
        setTimeout(() => this.updateStatus(), refreshPeriod);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {

  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
