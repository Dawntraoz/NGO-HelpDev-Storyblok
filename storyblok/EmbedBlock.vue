<script setup>
import { Column, Container, Row, Spinner } from '@papanasi/vue';

defineProps({ blok: Object });
</script>

<template>
  <Container class="embed">
    <Row class="embed__row">
      <Column basic="content">
        <iframe
          class="embed__frame"
          :width="blok.width"
          :height="blok.height"
          :src="`${blok.src}&${blok.params}`"
          frameborder="0"
          marginwidth="0"
          marginheight="0"
          allowfullscreen
          scrolling="no"
          webkitallowfullscreen
          mozallowfullscreen
          msallowfullscreen
        >
        </iframe>

        <Spinner full="true" class="embed__spinner" variant="primary" />
      </Column>
    </Row>
  </Container>
</template>

<style scoped>
.embed {
  margin: 0 auto;

  &__row {
    justify-content: center;
    position: relative;
    overflow: hidden;
    padding-top: 200%;
    position: relative;
    margin-bottom: 1rem;

    @media (--breakpoint-s) {
      padding-top: 100%;
    }
  }

  &__frame {
    position: relative;
    z-index: 1;
    border: 0;
    height: 200%;
    left: 0;
    position: absolute;
    top: 0;
    width: 100%;
    overflow: hidden;

    @media (--breakpoint-s) {
      height: 100%;
    }
  }

  &__spinner {
    z-index: 0;
  }
}
</style>
