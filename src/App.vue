<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <h2>{{ landscapeSubtitle }}</h2>
    <div style="width: 600px; height: 300px;" class="center">
      <vue-soccer-field orientation="landscape" :visitors="visitors" :receivers="receivers"/>
    </div>
    <br/>
    <h2>{{ portraitSubtitle }}</h2>
    <div style="width: 360px; height: 600px;" class="center">
      <vue-soccer-field orientation="portrait" :visitors="visitors" :receivers="receivers"/>
    </div>
    <h2>{{ systemSubtitle }}</h2>
    <div style="width: 800px; height: 400px;" class="center">
      <vue-soccer-field orientation="landscape" :visitors="visitors" :receivers="receivers" :home-system="systems.S433" :visitor-system="systems.S3421"/>
    </div>
    <br/>
    <h2>{{ colorSubtitle }}</h2>
    <div style="width: 400px; height: 800px;" class="center">
      <vue-soccer-field orientation="portrait" :visitors="visitors" :receivers="receivers" receiver-color="purple" visitor-color="orange" border-color="blue" player-border-color="yellow"/>
    </div>
    <br/>
    <h2>{{ unamedSubtitle }}</h2>
    <div style="width: 440px; height: 220px;" class="center">
      <vue-soccer-field orientation="landscape" :visitors="visitors" :receivers="receivers" :show-name="false"/>
    </div>
  </div>
</template>

<script>

import VueSoccerField from './component/vue-soccer-field'
import {SYSTEMS} from './enums/system'

export default {
  name: 'app',
  components: {VueSoccerField},
  comments: {
    VueSoccerField
  },
  data () {
    return {
      title: 'A Soccer Field vue component sample',
      systemSubtitle: 'Change teams system',
      colorSubtitle: 'Change teams color',
      unamedSubtitle: 'Whitout player name',
      portraitSubtitle: 'Default portait mode',
      landscapeSubtitle: 'Default landscape mode',
      receivers: [
        {number:1, name:'Yashin'},
        {number:2, name:'Maldini'},
        {number:3, name:'Cafu'},
        {number:4, name:'Beckenbauer'},
        {number:5, name:'Xavi'},
        {number:6, name:'Matthaus'},
        {number:7, name:'Christiano'},
        {number:8, name:'Pelé'},
        {number:9, name:'Ronaldo'},
        {number:10, name:'Maradona'},
        {number:11, name:'Messi'},
      ],
      visitors: [
        {number:1, name:'Yashin'},
        {number:2, name:'Maldini'},
        {number:3, name:'Cafu'},
        {number:4, name:'Beckenbauer'},
        {number:5, name:'Xavi'},
        {number:6, name:'Matthaus'},
        {number:7, name:'Christiano'},
        {number:8, name:'Pelé'},
        {number:9, name:'Ronaldo'},
        {number:10, name:'Maradona'},
        {number:11, name:'Messi'},
      ],
      systems: SYSTEMS,
    }
  }
}
</script>
<style lang="scss" scoped>

#app {
  text-align: center;
}

.center {
  margin-left: auto;
  margin-right: auto;
  margin-top: 3px;
  margin-bottom: 3px;
  text-align: center;
  display: flex;
  justify-content: center;
}

</style>