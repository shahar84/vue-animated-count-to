<script>
const easingFunctions = {
    // no easing, no acceleration
    linear: t => t,
    // accelerating from zero velocity
    easeInQuad: t => t * t,
    // decelerating to zero velocity
    easeOutQuad: t => t * (2 - t),
    // acceleration until halfway, then deceleration
    easeInOutQuad: t => (t < 0.5 ? 2 * t * t : -1 + (4 - 2 * t) * t),
    // accelerating from zero velocity
    easeInCubic: t => t * t * t,
    // decelerating to zero velocity
    easeOutCubic: t => --t * t * t + 1,
    // acceleration until halfway, then deceleration
    easeInOutCubic: t =>
        t < 0.5 ? 4 * t * t * t : (t - 1) * (2 * t - 2) * (2 * t - 2) + 1,
    // accelerating from zero velocity
    easeInQuart: t => t * t * t * t,
    // decelerating to zero velocity
    easeOutQuart: t => 1 - --t * t * t * t,
    // acceleration until halfway, then deceleration
    easeInOutQuart: t => (t < 0.5 ? 8 * t * t * t * t : 1 - 8 * --t * t * t * t),
    // accelerating from zero velocity
    easeInQuint: t => t * t * t * t * t,
    // decelerating to zero velocity
    easeOutQuint: t => 1 + --t * t * t * t * t,
    // acceleration until halfway, then deceleration
    easeInOutQuint: t =>
        t < 0.5 ? 16 * t * t * t * t * t : 1 + 16 * --t * t * t * t * t,
    easeOut: t => Math.pow(--t, 5) + 1
};
const defaultEasing = "easeOutQuad";

export default {
    name: 'AnimatedNumber', // vue component name
    props: {
        number: { default: 0 },
        duration: { default: 2000 },
        easing: { default: defaultEasing }
    },
    watch: {
        number(value) {
            const oldValue = this.displayNumber;
            const getProgress = ({ elapsed, total }) => Math.min(elapsed / total, 1);
            const easingFunction = easingFunctions.hasOwnProperty(this.easing)
                ? easingFunctions[this.easing]
                : easingFunctions[defaultEasing];
            const time = {
                start: performance.now(),
                total: this.duration
            };
            const tick = now => {
                if (this.displayNumber === this.number) return;
                time.elapsed = now - time.start;
                const progress = getProgress(time);
                const easing = easingFunction(progress);
                const change = (oldValue - value) * easing;
                this.displayNumber = Math.round(oldValue - change);
                if (progress < 1) requestAnimationFrame(tick);
            };
            requestAnimationFrame(tick);
        }
    },
    data() {
        return {
            displayNumber: 0,
        };
    },
    mounted() {
        this.displayNumber = this.number ? this.number : 0;
    }
}
</script>

<template>
    <div class="AnimatedNumber">
        {{displayNumber}}
    </div>
</template>

<style scoped>
    .AnimatedNumber {
        display: inline-block;
    }

</style>
