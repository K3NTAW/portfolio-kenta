<template>
    <div
        ref="loader"
        class="loader"
        style="
            position: fixed;
            top: 0;
            left: 0;
            z-index: 5;
            width: 100vw;
            height: 100vh;
            background-color: #030303;
            pointer-events: all;
        "
    >
        <p class="loader__word">
            <span
                v-for="(char, key) in firstWord"
                :key="key"
                ref="loaderWord1Chars"
                class="loader__word__char"
                >{{ char }}</span
            >
        </p>
        <p class="loader__word">and Welcome.</p>
        <p class="loader__word loader__word--logo serif">KW</p>
    </div>
</template>

<script>
export default {
    data: () => ({ firstWord: 'Hello' }),
    mounted() {
        this.$disableScrollY();

        const readTime = 0.45;
        const { loader, loaderWord1Chars } = this.$refs;
        const loaderChildren = Array.from(loader.children);
        const gsap = this.$gsap;

        const tl = gsap.timeline({
            delay: 0.6,
            paused: false,
            defaults: { duration: 0.65, ease: 'power1.out' }
        });

        tl.set(loaderChildren, {
            opacity: 0,
            filter: 'blur(10px)'
        });

        for (let i = 0; i < loaderChildren.length; i++) {
            const loaderChild = loaderChildren[i];

            if (i === 0) {
                tl.set(loaderChild, { opacity: 1, filter: 'blur(0px)' });
                tl.fromTo(
                    loaderWord1Chars,
                    { color: '#000' },
                    { color: '#fff', ease: 'power3.out', stagger: 0.05 }
                );
            } else {
                tl.to(
                    loaderChild,
                    { opacity: 1, filter: 'blur(0px)' },
                    '-=0.125'
                );
            }

            tl.to(
                loaderChild,
                { opacity: 0, filter: 'blur(5px)' },
                `+=${readTime}`
            );
        }

        tl.to(loader, {
            opacity: 0,
            onStart: () => {
                this.$enableScrollY();
            },
            onEnd: () => {
                this.$nuxt.$emit('show-shader');
                this.$nuxt.$emit('show-layout');
            }
        });

        tl.set(loader, { display: 'none' });
    }
};
</script>

<style lang="scss">
.loader {
    background-color: #030303;
    pointer-events: all;
    color: #dfdfdf;
    position: fixed;
    height: 100vh;
    width: 100vw;
    z-index: 5;
    left: 0;
    top: 0;

    &__word {
        transform: translate(-50%, -50%);
        font-size: var(--step-2);
        position: absolute;
        line-height: 1.125;
        overflow: hidden;
        opacity: 0;
        left: 50%;
        margin: 0;
        top: 50%;

        &__char {
            display: inline-block;
        }

        &--logo {
            font-size: var(--step-4);
        }
    }
}
</style>
