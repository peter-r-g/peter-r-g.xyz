<template>
  <nav>
    <div class="inner">
      <ul>
        <li>
          <nuxt-link to="/">Home</nuxt-link>
        </li>
        <li>
          <nuxt-link to="/projects">Projects</nuxt-link>
        </li>
      </ul>
    </div>
  </nav>
</template>

<style lang="scss" scoped>
@import "~assets/variables.scss";

ul {
  list-style: none;
  padding: 0;
  display: flex;

  li {
    margin: 0 20px;
    width: 125px;

    @media screen and (max-width: $screen-md) {
      margin: 0 10px;
    }
  }
}

nav {
  text-align: center;
  font-weight: bold;
  padding: 0;
  text-transform: uppercase;
  background-color: $dark-1000;

  @media (min-width: $screen-md) {
    justify-content: space-around;
  }

  .inner {
    display: flex;
    max-width: $max-width;
    width: 100%;
    align-items: center;
    margin: auto;
    justify-content: center;
    padding: 0 1rem;
  }
}

svg {
  margin-left: 8px;
}
</style>
