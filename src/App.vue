<template>
  <vue-soccer-field
          orientation="portrait"
          :visitors="visitors"
          :receivers="receivers"
          style="width: 360px; height: 600px;"
  />
</template>

<script>

  import VueSoccerField from './component/vue-soccer-field';

  export default {
    name: 'app',
    components: {VueSoccerField},
    comments: {
      VueSoccerField
    },
    data() {
      return {
        receivers: [
          {number: 1, name: 'Yashin'},
          {number: 2, name: 'Maldini'},
          {number: 3, name: 'Cafu'},
          {number: 4, name: 'Beckenbauer'},
          {number: 5, name: 'Xavi'},
          {number: 6, name: 'Matthaus'},
          {number: 7, name: 'Christiano'},
          {number: 8, name: 'Pelé'},
          {number: 9, name: 'Ronaldo'},
          {number: 10, name: 'Maradona'},
          {number: 11, name: 'Messi'},
        ],
        visitors: [
          {number: 1, name: 'Yashin'},
          {number: 2, name: 'Maldini'},
          {number: 3, name: 'Cafu'},
          {number: 4, name: 'Beckenbauer'},
          {number: 5, name: 'Xavi'},
          {number: 6, name: 'Matthaus'},
          {number: 7, name: 'Christiano'},
          {number: 8, name: 'Pelé'},
          {number: 9, name: 'Ronaldo'},
          {number: 10, name: 'Maradona'},
          {number: 11, name: 'Messi'},
        ],
      }
    }
  }
</script>