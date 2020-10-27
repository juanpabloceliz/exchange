<template>
    <button @click="buttonClick" class="bg-transparent px-1 hover:bg-green-500 text-green-600 font-semibold hover:text-white hover:border-none focus:border-none focus:shadow-none rounded">
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