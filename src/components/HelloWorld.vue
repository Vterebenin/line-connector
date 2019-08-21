<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      {{ someotherthing }}
      <svg height="100" width="100">
        <circle 
          cx="50" 
          cy="50" 
          r="40" 
          v-on:drag="debugMessage"
          stroke="black" 
          stroke-width="3" 
          fill="red" 
        />
      </svg>
    </p>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
    someotherthing: String
  },
  methods: {
    debugMessage() {
      console.log("test");
    }
  }
};
</script>

<style scoped>
</style>
