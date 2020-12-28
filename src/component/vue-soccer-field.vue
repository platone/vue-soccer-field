<template>
  <div class="ground" ref="ground" :style="groundStyle()">
    <div class="zone" :style="zoneStyle('home')">
      <div class="receiver-team">
        <div v-for="(player, index) in receivers" :key="index" class="player" :style="playerStyle(index, player, 'home', homeSystem)">
          <div class="player-number" :style="playerNumberStyle(index, player, 'home')">{{player.number}}</div>
          <div class="player-name" v-if="showName" :style="playerNameStyle()"><span>{{player.name}}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="penalty-area" :style="penaltyAreaStyle('home')">
      <div class="goal-area " :style="goalAreaStyle('home')">
      </div>
    </div>
    <div class="center-area" :style="circleStyle()"></div>
    <div class="penalty-area" :style="penaltyAreaStyle('visitor')">
      <div class="goal-area" :style="goalAreaStyle('visitor')">
      </div>
    </div>
    <div class="zone" :style="zoneStyle('visitor')">
      <div class="visitor-team">
        <div v-for="(player, index) in visitors" :key="index" class="player" :style="playerStyle(index, player, 'visitor', visitorSystem)">
          <div class="player-number" :style="playerNumberStyle(index, player, 'visitor')">{{player.number}}</div>
          <div class="player-name" v-if="showName" :style="playerNameStyle()">{{player.name}}</div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>

import {SYSTEMS} from "../enums/system";
import {ORIENTATION} from "../enums/orientation";

import backgroundUrl from '../assets/grass.png';

const ZONE = {
  RECEIVER : 'receiver',
  VISITOR : 'visitor',
}

const RATIO = {
  PENALTY : {
    LENGTH: 0.16,
    WIDTH: 0.61,
  },
  GOAL : {
    LENGTH: 0.05,
    WIDTH: 0.28,
  },
  CIRCLE : {
    WIDTH: 0.14,
  },
  PLAYER: {
    SIZE: 0.15,
  }
}

export default {
  name: "vue-soccer-field",
  props: {
    borderSize: {
      type: Number,
      default: 2
    },
    receiverColor: {
      type: String,
      default: '#3873b8'
    },
    visitorColor: {
      type: String,
      default: '#d61e00'
    },
    borderColor: {
      type: String,
      default: '#FFFFFF'
    },
    playerBorderColor: {
      type: String,
      default: '#FFFFFF'
    },
    playerBackgroundColor: {
      type: String,
      default: '#FFFFFF'
    },
    playerTextColor: {
      type: String,
      default: '#FFFFFF'
    },
    showName: {
      type: Boolean,
      default: true,
    },
    playerRatio: {
      type: Number,
      default: RATIO.PLAYER.SIZE
    },
    borderStyle: {
      type: String,
      default: 'solid'
    },
    homeSystem: {
      type: Array,
      default: function () {
        return SYSTEMS.S433;
      },
    },
    visitorSystem: {
      type: Array,
      default: function () {
        return SYSTEMS.S433;
      },
    },
    groundBackground: {
      type: String,
      default: backgroundUrl
    },
    externalSize: {
      type: [Number, String],
      default: 10
    },
    orientation: {
      type: [Number, String],
      default: ORIENTATION.PORTRAIT
    },
    receivers: {
      type: Array,
      default: []
    },
    visitors: {
      type: Array,
      default: []
    },
  },
  data() {
    return {
      ground: null,
      frames: {
        player: null,
      },
      systems: SYSTEMS,
      backgroundUrl,
    }
  },
  mounted() {
    this.ground = this.$refs.ground
  },
  created() {
  },
  computed: {
    client() {
      if(null !== this.ground) {
        return {
          w: this.$refs.ground.clientWidth,
          h: this.$refs.ground.clientHeight
        }
      }
      return {w: 0, h: 0}
    },
  },
  methods: {
    groundStyle() {
      let style =  { backgroundImage: `url(${this.groundBackground})` };
      Object.assign(style, { backgroundSize: `20%` });
      Object.assign(style, { backgroundPosition: `center center` });
      return style;
    },
    playerStyle(index, player, target, system) {
      const zone = {w:this.client.w - this.externalSize * 2 - this.borderSize * 2, h:this.client.h - this.externalSize * 2 - this.borderSize * 2, x:0, y:0};
      const frame = {w:zone.w * this.playerRatio, h:zone.h * this.playerRatio, x:0, y:0};
      const offset = {x: frame.w / 2, y: frame.h / 2};
      let style = `width: ${frame.w}px; height: ${frame.h}px;`;
      const position = system[index];
      switch (this.orientation) {
        case ORIENTATION.LANDSCAPE:
          switch (target) {
            case ZONE.VISITOR:
              style += `right:${zone.w / 2 * position[0] - offset.x}px;top:${zone.h * position[1] - offset.y}px;`;
              break;
            default:
              style += `left:${zone.w / 2 * position[0] - offset.x}px;top:${zone.h * position[1] - offset.y}px;`;
          }
          break;
        default:
          switch (target) {
            case ZONE.VISITOR:
              style += `left:${zone.w * position[1] - offset.x}px;bottom:${zone.h / 2 * position[0] - offset.y}px;`;
              break;
            default:
              style += `left:${zone.w * position[1] - offset.x}px;top:${zone.h / 2 * position[0] - offset.y}px;`;
          }
      }
      return style;
    },
    playerNameStyle() {
      const zone = {w:this.client.w - this.externalSize * 2 - this.borderSize * 2, h:this.client.h - this.externalSize * 2 - this.borderSize * 2, x:0, y:0};
      const frame = {w:zone.w * this.playerRatio, h:zone.h * this.playerRatio, x:0, y:0};
      const number = {w:Math.min(zone.w, zone.h) / 15, h:Math.min(zone.w, zone.h) / 15, x:0, y:0};
      const font = 10 + Math.min(zone.w, zone.h) / 15 / 4;
      let style = `width: ${frame.w}px; height: ${font}px;`;
      style += `position: absolute;`;
      style += `top: 50%;`;
      style += `margin-top: ${number.w / 2 + 4}px;`;
      style += `color: ${this.playerTextColor};text-align: bottom;line-height:100%;`;
      style += `z-index: 10;`;
      style += `font-size: ${font}px; font-weight: 400;`;
      return style;
    },
    playerNumberStyle(index, player, target) {
      const zone = {w:this.client.w - this.externalSize * 2 - this.borderSize * 2, h:this.client.h - this.externalSize * 2 - this.borderSize * 2, x:0, y:0};
      const frame = {w:Math.min(zone.w, zone.h) / 15, h:Math.min(zone.w, zone.h) / 15, x:0, y:0};
      const font = 8 + Math.min(zone.w, zone.h) / 15 / 4;
      let style = `width: ${frame.w}px; height: ${frame.h}px;`;
      style += `position: absolute;`;
      style += `top: 50%; left: 50%;`;
      style += `margin-top: -${frame.w / 2}px; margin-left: -${frame.h / 2}px;`;
      switch (target) {
        case ZONE.VISITOR:
          style += `background: ${this.visitorColor}; border: ${this.borderSize}px solid ${this.playerBorderColor};`;
          break;
        default:
          style += `background: ${this.receiverColor}; border: ${this.borderSize}px solid ${this.playerBorderColor};`;
      }
      style += `color: ${this.playerTextColor};text-align: center;line-height:${frame.h}px;`;
      style += `font-size: ${font}px; font-weight: bold;`;
      style += `padding: 0px;`;
      return style;
    },
    zoneStyle(target) {
      return `${this.zoneFrameStyleValue(target)}${this.borderStyleValue()}`;
    },
    penaltyAreaStyle(target) {
      return `${this.penaltyAreaStyleValue(target)}${this.borderStyleValue()}`;
    },
    goalAreaStyle(target) {
      return `${this.goalAreaStyleValue(target)}${this.borderStyleValue()}`;
    },
    circleStyle() {
      return `${this.circleStyleValue()}${this.borderStyleValue()}${this.radiusStyleValue()}`;
    },
    zoneFrameStyleValue(target) {
      const frame = {w:0, h:0, x:0, y:0};
      switch (this.orientation) {
        case ORIENTATION.LANDSCAPE:
          Object.assign(frame, { x: this.externalSize, y: this.externalSize });
          Object.assign(frame, { w: this.client.w / 2 - (this.externalSize + this.borderSize), h: this.client.h - (this.externalSize + this.borderSize) * 2 });
          switch (target) {
            case ZONE.VISITOR:
              return `width:${frame.w}px;height:${frame.h}px;top:${frame.y}px;right:${frame.x}px;`;
            default:
              return `width:${frame.w}px;height:${frame.h}px;top:${frame.y}px;left:${frame.x}px;`;
          }
        default:
          Object.assign(frame, { x: this.externalSize, y: frame.h + this.externalSize })
          Object.assign(frame, { w: this.client.w - (this.externalSize + this.borderSize) * 2, h: this.client.h / 2 - (this.externalSize + this.borderSize) });
          switch (target) {
            case ZONE.VISITOR:
              return `width:${frame.w}px;height:${frame.h}px;bottom:${frame.y}px;left:${frame.x}px;`;
            default:
              return `width:${frame.w}px;height:${frame.h}px;top:${frame.y}px;left:${frame.x}px;`;
          }
      }
    },
    penaltyAreaStyleValue(target) {
      const frame = {w:0, h:0, x:0, y:0};
      const center = {x:this.client.w / 2, y:this.client.h/ 2};
      switch (this.orientation) {
        case ORIENTATION.LANDSCAPE:
          Object.assign(frame, { w: this.client.w * RATIO.PENALTY.LENGTH - this.borderSize, h: this.client.h * RATIO.PENALTY.WIDTH - this.borderSize * 2 });
          switch (target) {
            case ZONE.VISITOR:
              return `width:${frame.w}px;height:${frame.h}px;top:${center.y - frame.h / 2}px;left:${this.externalSize}px;`;
            default:
              return `width:${frame.w}px;height:${frame.h}px;top:${center.y - frame.h / 2}px;right:${this.externalSize}px;`;
          }
        default:
          Object.assign(frame, { w: this.client.w * RATIO.PENALTY.WIDTH - this.borderSize, h: this.client.h * RATIO.PENALTY.LENGTH - this.borderSize * 2 });
          switch (target) {
            case ZONE.VISITOR:
              return `width:${frame.w}px;height:${frame.h}px;top:${this.externalSize}px;left:${center.x - frame.w / 2}px;`;
            default:
              return `width:${frame.w}px;height:${frame.h}px;bottom:${this.externalSize}px;left:${center.x - frame.w / 2}px;`;
          }
      }
    },
    goalAreaStyleValue(target) {
      const frame = {w:0, h:0, x:0, y:0};
      switch (this.orientation) {
        case ORIENTATION.LANDSCAPE:
          Object.assign(frame, { w: this.client.w * RATIO.GOAL.LENGTH - this.borderSize, h: this.client.h * RATIO.GOAL.WIDTH - this.borderSize * 2 });
          const landcapeCenter = { x: (this.client.w * RATIO.PENALTY.LENGTH - this.borderSize) / 2, y: (this.client.h * RATIO.PENALTY.WIDTH - this.borderSize * 2) / 2 };
          switch (target) {
            case ZONE.VISITOR:
              return `width:${frame.w}px;height:${frame.h}px;top:${landcapeCenter.y - frame.h / 2}px;left:${-this.borderSize}px;`;
            default:
              return `width:${frame.w}px;height:${frame.h}px;top:${landcapeCenter.y - frame.h / 2}px;right:${-this.borderSize}px;`;
          }
        default:
          Object.assign(frame, { w: this.client.w * RATIO.GOAL.WIDTH - this.borderSize, h: this.client.h * RATIO.GOAL.LENGTH - this.borderSize * 2 });
          const portraitCenter = { x: (this.client.w * RATIO.PENALTY.WIDTH - this.borderSize) / 2, y: (this.client.h * RATIO.PENALTY.LENGTH - this.borderSize * 2) / 2 };
          switch (target) {
            case ZONE.VISITOR:
              return `width:${frame.w}px;height:${frame.h}px;top:${-this.borderSize}px;left:${portraitCenter.x - frame.w / 2}px;`;
            default:
              return `width:${frame.w}px;height:${frame.h}px;bottom:${-this.borderSize}px;left:${portraitCenter.x - frame.w / 2}px;`;
          }
      }
    },
    circleStyleValue() {
      const frame = {w:0, h:0, x:0, y:0};
      const center = {x:this.client.w / 2, y:this.client.h/ 2};
      Object.assign(frame, { w: Math.min(this.client.w, this.client.h) * RATIO.CIRCLE.WIDTH, h: Math.min(this.client.w, this.client.h) * RATIO.CIRCLE.WIDTH });
      Object.assign(frame, { x: center.x - frame.w / 2 - this.borderSize, y: center.y - frame.h / 2 - this.borderSize});
      return `width:${frame.w}px;height:${frame.h}px;top:${frame.y}px;left:${frame.x}px;`;
    },
    borderStyleValue() {
      return `border: ${this.borderSize}px ${this.borderStyle} ${this.borderColor};`;
    },
    radiusStyleValue() {
      return 'border-radius: 50%;';
    },
  }
};
</script>
<style>

  .ground {
    background-color: #238729;
    width: 100%;
    height: 100%;
    position: relative;
  }

  .zone {
    position: absolute;
  }

  .penalty-area {
    position: absolute;
  }

  .goal-area{
    position: absolute;
  }

  .center-area {
    position: absolute;
  }

  .player {
    position: absolute;
    font-family: Questrial, sans-serif;
    text-align:center;
    display: inline;
    // background-color: coral;
  }

  .player-number {
    border-radius: 50%;
    text-align:center;
    display: table;
    margin: 0 auto;
  }

  .player-name {
    text-align:center;
    // background-color: aquamarine;
  }

</style>