<template>
    <section
        ref="projects"
        class="projects"
        aria-label="projects section"
        data-scroll-section
    >
        <V-H2 aria-label="project section heading">Swisscom Projects</V-H2>
        <ul class="projects__list" aria-label="projects list">
            <V-SwisscomProjects-Item
                v-for="(project, key) in projects"
                :key="key"
                ref="projectsListItems"
                :project="project"
                :aria-label="project.title"
                class="projects__list__item"
            />
        </ul>
    </section>
</template>

<script>
export default {
    data: () => ({
        projects: []
    }),
    created() {
        this.fetch(); // Call the fetch method when the component is created
    },
    methods: {
        async fetch() {
            this.projects = await this.$content('projects')
                .sortBy('createdAt', 'desc')
                .fetch();
        }
    }
};
</script>

<style lang="scss">
.projects {
    padding: 4rem clamp(1rem, 7vw, 5rem) 1rem;
    background-color: var(--black-color);
    pointer-events: all;
    margin-top: -1px;
    color: white;

    &__list {
        grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
        padding-inline-start: 0;
        justify-items: center;
        list-style-type: none;
        align-items: start;
        gap: var(--step-3);
        max-width: 1100px;
        display: grid;
        margin: 0 auto;

        @media screen and (min-width: 798px) {
            &__item:nth-child(even) {
                margin-block-start: 35%;
            }
        }
    }
}
</style>
