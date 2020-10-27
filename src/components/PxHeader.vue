<template>
    <header class="shadow w-full">
        <nav class="flex items-center justify-between flex-wrap bg-gray-900 p-6">
            <div class="flex items-center flex-shrink-0 text-white mr-6">
                <PxIcon class="mr-4" />
                <router-link class="font-semibold text-xl tracking-tight" to="/">Exchange</router-link>
            </div>
            <div class="hidden w-full flex-grow-0 lg:flex lg:items-center lg:w-auto">
                <div class="text-sm lg: flex-grow">
                    <router-link 
                        v-for="l in links"
                        :key="l.title"
                        :to="l.to" 
                        class="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4"
                    >
                    {{ l.title }}
                    </router-link>
                </div>
            </div>
        </nav>
    </header>
</template>

<script>
import PxIcon from '@/components/PxIcon';

export default {
    name: 'PxHeader',
    
    components: { PxIcon },

    props: {
        links: {
            type: Array,
            default: () => []
        }
    }
}
</script>