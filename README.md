# vue-soccer-field-sample

> A Soccer Field vue sample

## Installation

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).

## Usage

### In Vue Template

**Basic Usage**
```html
<vue-soccer-field 
	orientation="landscape|portrait" 
	:visitors="Array[{name:String, number:Number}]" 
	:receivers="Array[{name:String, number:Number}]"/>
```

**Example**
```html

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

```

### Value Binding
Use `visitors` and `receivers` to set list of differents players.


## Props

| Name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Type | Description |
| ---------------------- | :-------- | :--- |
| `border-size`          | `Number`  | Size of soccer field line. **default: 2**  |
| `receiver-color`       | `String`  | Background color of receiver player circle. **default: #3873b8**  |
| `visitor-color`  	     | `String`  | Background color of visitor player circle. **default: #d61e00**  |
| `border-color`  	     | `String`  | Color of soccer field line. **default: #FFFFFF**  |
| `player-border-color`  | `String`  | Border color of player circle. **default: #FFFFFF**  |
| `player-text-color`  	 | `String`  | Text color of player name. **default: #FFFFFF**  |
| `show-name`            | `Boolean` | Show player name. **default: true**  |
| `player-ratio`         | `Number`  | Ratio of circle. **default: 0.15**  |
| `border-style`         | `String`  | Border style of soccer field line. **default: solid**  |
| `receiver-system`      | `Array`   | Receiver soccer system. **default: SYSTEMS.S433**  (@see: system.js) |
| `visitor-system`       | `Array`   | Visitor soccer system. **default: SYSTEMS.S433**  (@see: system.js) |
| `ground-background`    | `String`  | Visitor soccer system. **default: assets/grass.png** |
| `external-size`        | `Number`  | Size of external zone. **default: 10** |
| `receivers`            | `Array`   | List of receiver player object with name and number ({name:String, number:Number}). **default: []** |
| `visitors`             | `Array`   | List of visitor player object with name and number ({name:String, number:Number}). **default: []** |
| `orientation`          | `String`  | Orientation of soccer field (landscape or portrait). **default: portrait**  (@see: orientation.js) |
