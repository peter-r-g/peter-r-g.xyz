<template>
  <BaseArea title="My Blog">
    <div class="container">
      <div class="post" v-for="item in items.items.slice(0, 6)" v-bind:key="item.title">
        <a :href="item.link" target="_blank">
          <div class="inner">
            <h3>{{ item.title }}</h3>
            <h4>{{ item.pubDate | date }}</h4>
            <p>
              {{ item.content }}
            </p>
            <div class="image"
              :style="`background-image: url(${item.enclosure ? item.enclosure.url : 'https://source.unsplash.com/random/?landscape'})`"
            ></div>

            <div class="read-post">
              <fa :icon="faChevronRight"></fa>

              Read post
            </div>
          </div>
        </a>
      </div>
    </div>

    <div class="view-more-container">
      <a target="_blank" class="view-more" href="https://blog.gu3.me/">View more <fa :icon="faUpRightFromSquare"></fa></a>
    </div>
  </BaseArea>
</template>

<script>
import { faChevronRight, faUpRightFromSquare } from "@fortawesome/free-solid-svg-icons";

export default {
  props: {
    items: Object
  },
  data: function() {
    return {
      faUpRightFromSquare,
      faChevronRight
    }
  }
}
</script>

<style lang="scss" scoped>
@import "~assets/variables.scss";
@import "@/assets/scrimMask.scss";

svg {
  margin-right: 4px;
}

.view-more-container {
  width: 100%;
  display: flex;
  justify-content: center;
}

.view-more {
  text-transform: uppercase;
  font-weight: bold;
  color: $dark-300;

  &:hover {
    color: $accent-500;
  }
}

.container {
  display: flex;
  justify-content: space-between;
  margin-top: 40px;
  margin-bottom: 40px;

  flex-direction: row;
  width: 100%;
  flex-wrap: wrap;

  gap: 10px;
}

.post {
  flex: 1 1 30%;

  @media screen and (max-width: $screen-md) {
    flex: 1 1 100%;
  }

  .inner {
    box-shadow: 0 0 20px rgba( black, 0.2 );
    width: 100%;
    z-index: -10;
    background: $dark-800;
    border-radius: 10px;
    padding: 20px;

    transition: all 150ms ease;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    border: 1px solid rgba( $dark-600, 0.2 );

    aspect-ratio: 1.25;
  }

  h3 {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  h4 {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  p {
    max-width: 300px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  .image {
    background-size: 125%;
    background-position: center;
    background-repeat: no-repeat;
    background-color: $dark-700;
    transition: all 150ms ease;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 130px;
    transform: scale( 1.0 );
  }

  .read-post {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    padding: 20px;
    background: $accent-500;

    text-transform: uppercase;
    font-weight: bold;

    padding: 0 20px;
    height: 0;

    transition: all 150ms ease;
  }
  
  &:hover { 
    .inner {
      border: 1px solid $accent-500;
      box-shadow: 0 0 20px rgba( $accent-500, 0.75 );
    }
    .read-post {
      background: $accent-500;

      padding: 20px;
      height: 60px;
    }

    .image {
    background-size: 135%;
    }
  }
}

</style>