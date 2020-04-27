<template>
  <div class="about">
    <div>
      <section>
        <b-select
          placeholder="Select a city"
          v-model="city"
          v-on:input="findblock()"
        >
          <option v-for="city in cities" :key="city">
            {{ city.name }}
          </option>
        </b-select>
        <b-select
          placeholder="Select a block"
          v-model="block"
          v-on:input="findlocation()"
        >
          <option v-for="block in blocks" :key="block">
            {{ block }}
          </option>
        </b-select>
        <b-select
          placeholder="Select a location"
          v-model="location"
          v-on:input="output()"
        >
          <option v-for="location in locations" :key="location">
            {{ location }}
          </option>
        </b-select>
      </section>
      <h1>{{ location }}</h1>
      <h2>溫度:{{ temp }}</h2>
      <h3>經緯度:{{ lat }},{{ lot }}</h3>
      <h3>觀測時間:{{ time }}</h3>
    </div>
  </div>
</template>
<script>
export default {
  name: 'home',
  data() {
    return {
      time: '',
      lat: '',
      lot: '',
      temp: '',
      locations: [],
      blocks: [],
      api:
        'https://opendata.cwb.gov.tw/api/v1/rest/datastore/O-A0001-001?Authorization=rdec-key-123-45678-011121314',
      cities: [
        { name: '基隆市' },
        { name: '臺北市' },
        { name: '新北市' },
        { name: '桃園市' },
        { name: '新竹市' },
        { name: '新竹縣' },
        { name: '苗栗縣' },
        { name: '臺中市' },
        { name: '彰化縣' },
        { name: '南投縣' },
        { name: '雲林縣' },
        { name: '嘉義市' },
        { name: '嘉義縣' },
        { name: '臺南市' },
        { name: '高雄市' },
        { name: '屏東縣' },
        { name: '宜蘭縣' },
        { name: '花蓮縣' },
        { name: '臺東縣' },
        { name: '澎湖縣' },
        { name: '金門縣' },
        { name: '連江縣' }
      ]
    }
  },
  methods: {
    findblock() {
      this.blocks = []
      this.locations = []
      this.axios.get(this.api).then((res) => {
        for (let n = 0; n <= 438; n++) {
          if (
            this.city ==
            res.data['records']['location'][n]['parameter']['0'][
              'parameterValue'
            ]
          ) {
            if (
              this.blocks.indexOf(
                res.data['records']['location'][n]['parameter']['2'][
                  'parameterValue'
                ]
              ) == -1
            )
              this.blocks.push(
                res.data['records']['location'][n]['parameter']['2'][
                  'parameterValue'
                ]
              )
          }
        }
      })
    },
    findlocation() {
      this.locations = []
      this.axios.get(this.api).then((res) => {
        for (let n = 0; n <= 438; n++) {
          if (
            this.block ==
            res.data['records']['location'][n]['parameter']['2'][
              'parameterValue'
            ]
          ) {
            if (
              this.city ==
              res.data['records']['location'][n]['parameter']['0'][
                'parameterValue'
              ]
            ) {
              this.locations.push(
                res.data['records']['location'][n]['locationName']
              )
            }
          }
        }
      })
    },
    output() {
      this.message = ''
      this.axios.get(this.api).then((res) => {
        for (let n = 0; n <= 438; n++) {
          if (
            this.location == res.data['records']['location'][n]['locationName']
          ) {
            this.time = res.data['records']['location'][n]['time']['obsTime']
            this.lat = res.data['records']['location'][n]['lat']
            this.lot = res.data['records']['location'][n]['lat']
            this.temp =
              res.data['records']['location'][n]['weatherElement']['3'][
                'elementValue'
              ]
          }
        }
      })
    }
  }
}
</script>
<style></style>
