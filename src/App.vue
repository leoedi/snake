<template>
  <div @keydown.enter="downes">
    <button v-if="show" class="btn" ref="btn" @click="methodss">start</button>
    <div
      ref="pixel"
      :style="{ left: left + 'px', downs: downs + 'px' }"
      class="pixel"
    ></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      left: 0,
      right: null,
      top: 0,
      downs: null,
      show: true,
    };
  },
  methods: {
    methodss() {
      this.show = false;
      setInterval(() => (this.left += 1), 50);
    },
    downes() {
      console.log("dashk");
    },
  },
};
</script>
<style>
.pixel {
  position: absolute;
  width: 20px;
  height: 20px;
  background: #000;
}
.btn {
  position: absolute;
  right: 0px;
  direction: rtl;
}
</style>
