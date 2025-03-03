<template>
    <header ref="header" class="header" role="banner" data-scroll-section>
        <V-Header-Background class="header__canvas" />

        <div ref="headerContainer" class="header__container">
            <h1 class="header__container__title">
                <span class="sr-only">Kenta Waibel</span>
                <span class="line" aria-hidden="true">
                    <span class="line__content">Kenta</span>
                </span>
                <span class="line" aria-hidden="true">
                    <span class="line__content ml-responsive serif">
                        Waibel
                    </span>
                </span>
            </h1>

            <div class="header__container__subtitle">
                <p class="sr-only">{{ subTitleText }}</p>
                <!-- eslint-disable -->
                <span
                    v-for="(char, key) in subTitleText"
                    :key="key"
                    ref="headerContainerSubtitle"
                    aria-hidden="true"
                    v-html="char"
                />
                <!-- eslint-enable -->
            </div>
        </div>

        <div class="header__bottom-bar">
            <!-- eslint-disable -->
            <V-Scroll-Down
                class="header__bottom-bar__scroll-down"
                @click.native="$scrollTo('.works')"
            />
            <!-- eslint-enable -->
        </div>
    </header>
</template>

<script>
export default {
    data() {
        return {
            links: [
                { label: 'Work', action: () => this.$scrollTo('.works') },
                { label: 'About', action: () => this.$scrollTo('.about') },
                { label: 'Contact', action: () => this.$scrollTo('.contact') }
            ],
            subTitleText: 'Developer Apprentice'
        };
    },
    mounted() {
        const { header, headerContainer, headerContainerSubtitle } = this.$refs;
        const gsap = this.$gsap;

        const prefersReducedMotion = this.$prefersReducedMotion();
        if (!prefersReducedMotion) {
            gsap.fromTo(
                headerContainer,
                { y: -3 },
                {
                    y: 20,
                    scrollTrigger: {
                        scrub: true,
                        trigger: header,
                        start: 'top-=50px top',
                        end: 'bottom top'
                    }
                }
            );

            gsap.fromTo(
                '.scroll-down',
                { opacity: 1 },
                {
                    opacity: 0,
                    scrollTrigger: {
                        scrub: 0.75,
                        trigger: header,
                        start: 'top+=25% top',
                        end: 'bottom-=25% top'
                    }
                }
            );
        }

        const tl = gsap.timeline({
            paused: true,
            delay: 0.25,
            onEnd: () => this.$locomotiveScroll.update()
        });

        if (prefersReducedMotion) {
            tl.from('.line__content', { opacity: 0, stagger: 0.25 });
        } else {
            tl.from('.line__content', {
                yPercent: 105,
                ease: 'power1.out',
                duration: 1,
                stagger: 0.25
            });
        }

        tl.from(
            headerContainerSubtitle,
            { opacity: 0, stagger: { amount: 0.5, from: 'center' } },
            '-=0.75'
        );

        tl.from(
            '.nav__sections__list__section, .nav__menu-button',
            { opacity: 0, stagger: 0.05 },
            '<+0.75'
        );

        tl.from('.scroll-down', { opacity: 0 }, '<+0.25');
        this.$nuxt.$on('show-layout', () => tl.play());
    }
};
</script>

<style lang="scss">
.header {
    --100vh: calc(100 * var(--vh, 1vh));

    background-color: var(--black-color);
    justify-content: center;
    align-items: center;
    pointer-events: all;
    position: relative;
    min-height: 100vh;
    overflow: hidden;
    max-width: 100vw;
    display: flex;
    width: 100%;
    z-index: 0;

    &__canvas {
        position: absolute;
        z-index: -10;
        height: 100%;
        width: 100%;
        top: 0;
        left: 0;
    }

    &__container {
        mix-blend-mode: difference;
        cursor: default;
        color: white;

        &__title {
            margin-bottom: 1rem;
            line-height: 1.2;

            & > *:last-of-type {
                margin-top: min(-0.5rem, calc(1vw * -1));
            }
        }

        &__subtitle {
            font-size: var(--step--1);
            text-align: center;
        }
    }

    &__bottom-bar {
        top: calc(var(--100vh) - var(--step--1));
        transform: translateY(-100%);
        justify-content: flex-start;
        right: var(--x-padding);
        left: var(--x-padding);
        align-items: flex-end;
        position: absolute;
        display: flex;

        &__clock {
            color: lighten($color: grey, $amount: 1);
            display: none;
        }

        &__scroll-down {
            color: lighten($color: grey, $amount: 7);
        }
    }

    &__navigation {
        padding-inline: clamp(1rem, 4vw, 5rem);
        mix-blend-mode: difference;
        justify-content: flex-end;
        font-size: var(--step--1);
        list-style-type: none;
        pointer-events: all;
        align-items: center;
        position: absolute;
        display: flex;
        color: white;
        width: 100%;
        left: 0;
        top: 0;

        & > * {
            position: relative;
        }

        @media screen and (max-width: 512px) {
            justify-content: space-between;
            width: 100%;
        }
    }

    &::before {
        position: absolute;
        max-height: 11rem;
        min-height: 2rem;
        content: '';
        width: 100%;
        height: 25%;
        z-index: 0;
        bottom: 0;
        left: 0;

        background: linear-gradient(
            to top,
            var(--black-color) 0%,
            var(--black-color) 25%,
            rgba(3, 3, 3, 0)
        );
    }
}

.line {
    overflow: hidden;
    display: block;

    &__content {
        display: inline-block;
    }
}
</style>
