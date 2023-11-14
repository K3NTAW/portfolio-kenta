<template>
    <li ref="project" v-hoverable class="project">
        <a
            :href="project.live"
            :aria-label="`open live ${project.title}`"
            target="_blank"
            rel="noopener noreferrer"
            class="project__wrapper"
        >
            <div ref="projectImageWrapper" class="project__image__wrapper">
                <nuxt-img
                    ref="projectImage"
                    :src="project.image"
                    :alt="project.title"
                    class="project__image"
                    format="webp"
                    sizes="sm:500 md:900 lg:100%"
                    quality="90"
                />
            </div>
            <div class="project__content">
                <h3 class="project__content__title">
                    {{ project.title }}
                </h3>
                <p class="project__content__tags">
                    {{ tagsString }}
                </p>
            </div>
        </a>
        <a
            v-if="project.source"
            ref="projectSource"
            :href="project.source"
            :aria-label="`open source of ${project.title}`"
            target="_blank"
            rel="noopener noreferrer"
            class="project__source"
            title="source"
        >
            <SourceSVG />
        </a>
    </li>
</template>

<script>
import SourceSVG from '~/images/logo/GitHub.svg?inline';

export default {
    components: { SourceSVG },
    props: { project: { type: Object, required: true, default: () => ({}) } },
    computed: {
        tagsString() {
            return this.project.tags.reduce((str, val) => `${str}, ${val}`);
        }
    },
    mounted() {
        const prefersReducedMotion = this.$prefersReducedMotion();
        const { project, projectImage, projectSource, projectImageWrapper } =
            this.$refs;
        const gsap = this.$gsap;
        const imageResizeObserver = new ResizeObserver(() => {
            if (!projectSource) return;
            gsap.to(projectSource, {
                '--image-height': `${projectImageWrapper.clientHeight}px`
            });
        });

        imageResizeObserver.observe(projectImage.$el);
        gsap.fromTo(
            project,
            { opacity: 0, y: prefersReducedMotion ? 0 : 100 },
            {
                opacity: 1,
                y: 0,
                ease: 'power4.out',
                duration: 0.75,
                scrollTrigger: {
                    trigger: project,
                    start: 'top bottom-=10%',
                    once: true
                }
            }
        );

        const imageMovementBase = 3.5;
        const randomMultiplier = gsap.utils.random(0.85, 1.15);
        const imageMovement = imageMovementBase * randomMultiplier;

        gsap.fromTo(
            projectImage.$el,
            { yPercent: -50 + imageMovement * -1 },
            {
                yPercent: -50 + imageMovement,
                scrollTrigger: { trigger: project, scrub: true }
            }
        );
    }
};
</script>

<style lang="scss">
@use 'sass:color';

.project {
    will-change: transform, opacity;
    position: relative;
    max-width: 475px;

    &__wrapper {
        grid-template-rows: fit-content fit-content;
        text-decoration: none;
        display: grid;
        cursor: none;
        gap: 1rem;

        @media (prefers-reduced-motion: reduce) {
            cursor: pointer;
        }
    }

    &__image {
        object-position: center center;
        will-change: transform;
        position: relative;
        object-fit: cover;
        height: 110%;
        width: 100%;
        top: 50%;
        left: 0;

        &__wrapper {
            overflow: hidden;
            min-height: 20rem;
            max-height: 30rem;
            height: 40vw;
            width: 100%;

            @supports (aspect-ratio: 1/1) {
                aspect-ratio: 1/1;
                max-height: unset;
                min-height: unset;
                height: unset;
            }
        }
    }

    &__content {
        $text-color: color.adjust(
            $lightness: -20%,
            $color: #fff
        );

        &__title {
            font-size: calc(var(--step-0) + 0.4rem);
            letter-spacing: 0.25px;
            color: $text-color;
            margin-bottom: 0;
        }

        &__tags {
            font-size: calc(var(--step--2) - 0rem);
            color: darken($text-color, 5);
        }
    }

    &__source {
        --pd: 0.5rem;

        color: darken($color: white, $amount: 10);
        top: calc(var(--image-height) - var(--pd));
        transform: translateY(-100%);
        max-height: var(--step-3);
        max-width: var(--step-3);
        mix-blend-mode: screen;
        position: absolute;
        right: var(--pd);
        min-height: 48px;
        min-width: 48px;
        cursor: none;
        z-index: 1;
    }

    &::after {
        background-color: rgba($color: #030303, $alpha: 0.35);
        transition: opacity 400ms;
        pointer-events: none;
        position: absolute;
        content: '';
        z-index: 10;
        bottom: 0;
        right: 0;
        left: 0;
        top: 0;
    }

    &:is(:focus-within, :hover) {
        &::after {
            opacity: 0;
        }
    }
}
</style>
