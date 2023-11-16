<template>
    <header ref="header" class="header" role="banner" data-scroll-section>
        <V-SwisscomProjectsHeader-Background class="header__canvas" />

        <div ref="headerContainer" class="header__container">
            <h1 class="header__container__title">
                <span class="sr-only">Swisscom Projects</span>
                <span class="line" aria-hidden="true">
                    <span class="line__content">Swisscom</span>
                </span>
                <span class="line" aria-hidden="true">
                    <span class="line__content ml-responsive serif">
                        Projects
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
            subTitleText: 'My Projects at Swisscom AG'
        };
    },
    mounted() {},
    methods: {}
};
</script>

<style lang="scss"></style>
