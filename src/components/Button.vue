<template>
  <button :style="{ background: color }" class="btn" @click="onClick()">{{text}}</button>
</template>

<script>
export default {
    // eslint-disable-next-line
    name: 'Button',
    props: {
        text: String,
        color: String
    },
    methods: {
        onClick() {
            console.log('Click')
        }
    }
}

</script>

<style>

</style>