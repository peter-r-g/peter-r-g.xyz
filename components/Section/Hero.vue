<template>
  <div class="hero-wrapper">
    <article class="hero">

        <div class="hero-image"></div>

        <h1>Peter Gorman</h1>
        <social-links></social-links>

        <hr/>

        <p>👋 Hey! I'm Peter.</p>
        <p>I am a software engineer from the United Kingdom, now living in Canada! <wbr>
          I work on open source projects in the game development space.</p>
        <hr/>

    </article>
  </div>
</template>

<script>
import { faArrowDown } from "@fortawesome/free-solid-svg-icons";
export default {
  data: function() {
    return {
      faArrowDown
    }
  }
}
</script>

<style lang="scss" scoped>
@import "~assets/variables.scss";
@import "@/assets/scrimMask.scss";

.hero {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;

  margin: 0 auto;
  padding: 20px 1rem;

  flex-direction: column;

  max-width: calc( $max-width / 1.5 );
  text-align: center;

  padding-bottom: 40px;

  min-height: 66vh;

  @media (max-width: 768px) {
    height: calc( 100vh - 25px );
  }
}

.hero-wrapper {
  background: $dark-1000;
}

.hero-image {
  overflow: hidden;
  width: 256px;
  aspect-ratio: 1;
  background-image: url( '/me.png' );
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  border-radius: 100%;

  @media (max-width: 768px) {
    width: 128px;
  }
}

h1 {
  position: relative;
  width: fit-content;
  margin: 5px 0;
  margin-top: 10px;
}
</style>
