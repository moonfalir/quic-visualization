<template>
    <rect width="9" height="9" v-bind:fill="fillcolor" v-bind:transform="'translate(' + translateX + ', ' + translateY + ')'" @click="selectpacket()" 
    stroke="black" v-bind:stroke-width="strokewidth"/>
</template>

<script lang="ts">
export default {
  name: "packetblock",
  props: ['packetinfo', 'traceid', 'connid', 'packetid'],
  data() {
      return {
          translateY: 10
      }
  },
  computed: {
    fillcolor(){
        return this.$store.state.framecolortables.getFrameColour(this.packetinfo.frametype)
    },
    translateX() {
        return this.$store.state.timescalestate.calcTranslateX((this.packetinfo.timestamp * 1000) + parseInt(this.xoffset))
    },
    strokewidth(){
        if (this.$store.state.vissettings.getFile(this.traceid).getConn(this.connid).isPacketSelected(this.packetid))
            return 2
        else
            return 0
    },
    xoffset(){
        return this.$store.state.vissettings.getFile(this.traceid).getConn(this.connid).getXOffset()
    }
  },
  methods: {
    selectpacket: function(){
        let data = {
            traceid: this.traceid, 
            connid: this.connid, 
            packetid: this.packetid
        }
        this.$store.dispatch('setSelectedPacket', data)
    }
  }
};
</script>

<style>
</style>