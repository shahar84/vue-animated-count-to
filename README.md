# Vue Animated Count To Component

This is an easy to use count to component for vue js, with zero dependencies.

## Installation
You can install the package via npm:
```bash
npm install vue-animated-count-to --save
```

or yarn:

```bash
yarn add vue-animated-count-to
```

Next, you must register the component. The most common use case is to do that globally.
```js
//in your app.js or similar file
import Vue from 'vue';
import AnimatedCountTo from 'vue-animated-count-to';

Vue.component('animated-count-to', AnimatedCountTo);
```


```vue
<template>
  <animated-count-to :number='number' easing='easeInOutQuint' :duration='3000'/>
</template>
<script>
import AnimatedCountTo from 'vue-animated-count-to';
export default {
  components: { AnimatedCountTo },
  data () {
    return {
      number: 200,
    }
  }
}
</script>
``` 


