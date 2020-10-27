<template>
    <button @click="buttonClick" class="bg-transparent hover:bg-green-500 text-green-700 font-semibold hover:text-white hover:border-transparent focus:border-transparent rounded">
        <beat-loader :loading="isLoading" :color="'#68d391'" :size="8" />
        <span v-show="!isLoading">
            <slot></slot>
        </span>
    </button>
</template>

<script>
export default {
    name: 'Pxbutton',

    props: {
        isLoading: {
            type: Boolean,
            default: false
        }
    },

    methods: {
        buttonClick () {
            this.$emit('click')
        }
    }
}
</script>