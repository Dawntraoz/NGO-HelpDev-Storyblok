<script setup>
import { Button, Column, Container, Row } from '@papanasi/vue';
import { useSettingsStore } from '~/stores';

const storyblokApi = useStoryblokApi();
const { locale } = useI18n();
const store = useSettingsStore();

const { data } = await storyblokApi.get('cdn/stories/config', {
  version: 'draft',
  resolve_links: 'url',
  language: locale.value
});

store.update(data.story.content);

useHead({
  htmlAttrs: {
    lang: locale.value
  },
  title: store.title,
  meta: [{ name: 'description', content: store.description }],
  script: [
    {
      hid: 'gtm',
      children: data.story.content.gtag
        ? `(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
      new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
      j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
      'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
      })(window,document,'script','dataLayer','${data.story.content.gtag}');`
        : '',
      type: 'text/javascript'
    }
  ]
});

const menu = ref(store.menu);
const actions = ref(store.actions);
</script>

<template>
  <Container :fluid="true" class="header">
    <Row basic="column" s="row" class="header__container">
      <Column :s="2" class="header__column">
        <NuxtLink to="/">
          <img
            class="header__logo"
            src="~/assets/images/logo_landscape_white.png"
            :alt="data.story.content.title"
            width="200"
            height="30"
          />
        </NuxtLink>
      </Column>
      <Column class="header__column">
        <nav v-if="menu" class="header__nav">
          <ul class="header__menu">
            <li v-for="blok in menu" :key="blok._uid">
              <NuxtLink
                class="header__link"
                :target="blok.link.target"
                :to="
                  blok.link.cached_url.includes('index')
                    ? blok.link.cached_url.replace('index', locale === 'en' ? '/' : '')
                    : blok.link.cached_url
                "
              >
                {{ blok.title }}
              </NuxtLink>
            </li>
          </ul>
        </nav>
      </Column>
      <Column :s="2" class="header__column">
        <ul class="header__menu">
          <li v-for="blok in actions" :key="blok._uid">
            <NuxtLink
              class="header__link"
              :target="blok.link.target"
              :to="
                blok.link.cached_url.includes('index')
                  ? blok.link.cached_url.replace('index', locale === 'en' ? '/' : '')
                  : blok.link.cached_url
              "
            >
              <Button class="header__button" :outline="true" variant="basic">{{ blok.title }}</Button>
            </NuxtLink>
          </li>
        </ul>
      </Column>
    </Row>
  </Container>
</template>

<style scoped>
.header {
  position: absolute;
  top: 1rem;
  height: var(--header-height-min);

  @media (--breakpoint-s) {
    height: var(--header-height);
    top: 0;
  }

  &__container {
    justify-content: center;
    align-items: center;
    margin: 0 auto;
    max-width: var(--pa-grid-container-max-width);
    width: 90vw;
    height: var(--header-height-min);

    @media (--breakpoint-s) {
      height: var(--header-height);
    }
  }

  &__logo {
    max-width: 8rem;
    height: auto;

    @media (--breakpoint-s) {
      max-width: 13rem;
    }
  }

  &__column {
    justify-content: center;
    align-items: center;
  }

  &__nav,
  &__menu {
    padding: 0;
    margin: 0;
    width: 100%;
  }

  &__menu {
    display: flex;
    justify-content: center;
    list-style: none;
  }

  &__link {
    color: var(--color-basic-brightest);
    font-family: var(--font-family-heading);
    font-weight: var(--font-weight-medium);
    text-align: center;
    transition: color var(--transition-duration-normal);
    max-width: 80%;
    margin: 0 1rem;

    &:hover {
      color: var(--color-primary-brightest);
    }

    &.router-link-active {
      text-decoration: underline;
    }

    @media (--breakpoint-s) {
      font-size: var(--font-size-l);
      margin: 0 2rem;
    }
  }

  &__button {
    --pa-border-width-small: 3px;
    --pa-button-radius: 100rem;

    background-color: transparent;
    font-size: var(--font-size-s);

    color: var(--color-basic-brightest);
    border-color: var(--color-basic-brightest);
    transition: opacity var(--transition-duration-normal);

    @media (--breakpoint-s) {
      font-size: var(--font-size-m);
    }

    &:hover {
      opacity: 0.9;
    }
  }
}
</style>
