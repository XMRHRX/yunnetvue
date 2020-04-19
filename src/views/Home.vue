<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
	<input type="text" v-model="locationName" placeholder="請輸入地點">
	<button @click="local">Show location</button>
	<h1>
	{{message}}
	</h1>
	<pre>{{result}}</pre>
  </div>
</template>

<script>
// @ is an alias to /src
export default {
  name: "Home",
  data() {
    return {
      message: "",
      locationName: "",
	  result: "",
    };
  },
  methods: {
    getMessage() {
      return "Hi I am a sync message";
    },
    asyncMessage() {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve("I am an async message");
        }, 1000);
      }).then((res) => {
        this.message = res;
      });
    },
    local() {
      let api = "https://opendata.cwb.gov.tw/api/v1/rest/datastore/O-A0001-001?Authorization=rdec-key-123-45678-011121314";
	  if(this.locationName == "")
	  {
	  	alert("please input location");
	  	return;
	  }
	  api+=`&locationName=${this.locationName}`;
      return this.axios.get(api).then((res) => {
        let weatherData = res.data['records']['location']['0']['weatherElement'];
		this.result="";
		for(var row in weatherData)
		{
			this.result += weatherData[row]["elementName"]+":"+weatherData[row]['elementValue'] + "\n";
		}
        /*this.result = ;*/
      });
    }
  }
};
</script>
