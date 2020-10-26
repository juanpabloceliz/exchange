<template>
    <header class="shadow w-screen">
        <nav class="flex items-center justify-between flex-wrap bg-green-400 p-6">
            <div class="flex items-center flex-shrink-0 text-white mr-6">
                <PxIcon class="mr-4" />
                <span class="font-semibold text-xl tracking-tight">Exchange</span>
            </div>
            <div class="hidden sm:block w-full md:block flex-grow-0 lg:flex lg:items-center lg:w-auto">
                <div class="text-sm lg: flex-grow">
                    <router-link to="/about" class="mt-5 text-xl text-green-600 hover:underline">About</router-link>
                </div>
            </div>
        </nav>
    </header>
</template>

<script>
import PxIcon from '@/components/PxIcon';

export default {
    name: 'PxHeader',
    components: { PxIcon }
}
</script>