<template>
  <div>
    <h1>Choose two objects:</h1>
    <div id="form-container">
      <input
              v-model="innerText"
              placeholder="Object A"
              :style="{
                backgroundColor:this.$store.state.color2,
                fontSize:this.$store.state.objectsFontSize
               }"
      >
      <span style="margin: 0 20px;"></span>
      <input
              v-model="outerText"
              placeholder="Object B"
              :style="{
                backgroundColor:this.$store.state.color1,
                fontSize:this.$store.state.objectsFontSize
              }"
      >
    </div>
    <h1>Select a diagram</h1>
    <div id="stage-container" v-bind:style="styleObject">
      <v-stage :config="configKonva">
        <v-layer :config="configLayer">
          <ThreePossibilities
                  :x="this.configKonva.width/2 - baseRadius*1.2"
                  v-bind:y="baseRadius"
                  v-bind:radius="baseRadius"
                  :outerText="outerText"
                  :innerText="innerText"
                  pervasionName="SubjectInsidePredicate"
                  :swapColors="false"
          />
          <ThreePossibilities
                  :x="this.configKonva.width/2 + baseRadius*1.2"
                  v-bind:y="baseRadius"
                  v-bind:radius="baseRadius"
                  :outerText="innerText"
                  :innerText="outerText"
                  pervasionName="PredicateInsideSubject"
                  :swapColors="true"
          />
          <MutuallyExclusive
                  :x="baseRadius"
                  v-bind:y="baseRadius*3.2"
                  v-bind:radius="baseRadius/1.5"
                  :outerText="innerText"
                  :innerText="outerText"
          />
          <OneWith
                  :x="baseRadius*4"
                  v-bind:y="baseRadius*3.2"
                  v-bind:radius="baseRadius/1.5"
                  :outerText="innerText"
                  :innerText="outerText"
          />
          <FourPossibilities
                  :x="baseRadius*5.7"
                  v-bind:y="baseRadius*3.2"
                  v-bind:radius="baseRadius/1.5"
                  :outerText="innerText"
                  :innerText="outerText"
          />
        </v-layer>
      </v-stage>
    </div>
  </div>
</template>

<script>

import ThreePossibilities from './ThreePossibilities'
import MutuallyExclusive from './MutuallyExclusive'
import OneWith from './OneWith'
import FourPossibilities from './FourPossibilities'

export default {
  name: 'Screen1',
  components: {
    ThreePossibilities,
    MutuallyExclusive,
    OneWith,
    FourPossibilities
  },
  computed: {
    innerText: {
      get () { return this.$store.state.subject},
      set (value) {
        this.$store.commit('setSubject', value);
        if (!value) this.$store.commit('setSelectedPervasion', null);
      }
    },
    outerText: {
      get () { return this.$store.state.predicate},
      set (value) {
        this.$store.commit('setPredicate', value);
        if (!value) this.$store.commit('setSelectedPervasion', null);
      }
    }
  },
  methods: {
    handleResize() {
      let width = window.innerWidth;
      console.log("width:" + width);
      let scale = width*0.8 / 800;
      this.styleObject.width = width*0.8 + 'px';
      this.configKonva = {
        width: this.stageWidth * scale,
        height: this.stageHeight * scale,
        scale: {
          x: scale,
          y: scale
        }
      }
    }
  },
  beforeMount: function () {
    window.addEventListener('resize', this.handleResize)
  },
  beforeDestroy: function () {
    window.removeEventListener('resize', this.handleResize)
  },
  data() {

    const stageWidth = this.$store.state.stageWidth;
    const stageHeight = 400;

    //console.log("stageWidth:" + stageWidth);

    return {
      stageWidth: this.$store.state.stageWidth,
      stageHeight: stageHeight,
      styleObject: {
        width: stageWidth + 'px',
        margin: '0 auto'
      },
      configKonva: {
        width: stageWidth,
        height: stageHeight,
        scale: {
          x:1,
          y:1
        }
      },
      configLayer: {
      },
      baseRadius: stageWidth/8
    }
  }
}
</script>

<style lang="scss">

  #form-container {
    min-width:800px;
    margin-bottom: 30px;
    font-size: 18px;
    input {
      padding:5px;
      border-style: hidden;
      text-align: center;
    }
  }

</style>
