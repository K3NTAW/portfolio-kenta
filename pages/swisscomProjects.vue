<template>
    <div role="presentation">
        <V-SwisscomProjectsNavbar :current-section="currentSection" />
        <SwisscomProjectsHeader aria-label="content header" />
        <V-SwisscomProjects />
        <main aria-label="main content"></main>
        <V-Footer />
        <V-Menu :current-section="currentSection" />
    </div>
</template>
<script>
import SwisscomProjectsHeader from '@/components/V-SwisscomProjectsHeader.vue'; // Adjust the path accordingly

export default {
    name: 'SwisscomProjects',
    components: {
        SwisscomProjectsHeader
    },
    layout: 'swisscomProjectsLayout',
    data: () => ({ currentSection: 0 }),
    mounted() {
        const gsap = this.$gsap;
        const ScrollTrigger = this.$ScrollTrigger;

        gsap.utils.toArray('header, section').forEach((item, i) => {
            ScrollTrigger.create({
                trigger: item,
                start: 'top 45%',
                end: 'bottom 45%',
                onEnter: () => (this.currentSection = i),
                onEnterBack: () => (this.currentSection = i)
            });
        });
    }
};
</script>
