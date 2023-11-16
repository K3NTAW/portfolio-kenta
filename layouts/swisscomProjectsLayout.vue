<template>
    <div>
        <div role="presentation" class="scroller">
            <Nuxt />
        </div>
        <V-Pointer />
        <V-SwisscomProjectsLoader />
    </div>
</template>

<script>
export default {
    name: 'SwisscomProjectsLayout',
    mounted() {
        this.setViewHeight();
        window.addEventListener('resize', this.setViewHeight);
    },
    methods: {
        setViewHeight() {
            const vh = window.innerHeight * 0.01;
            document.documentElement.style.setProperty('--vh', `${vh}px`);
        }
    }
};
</script>

<style></style>
