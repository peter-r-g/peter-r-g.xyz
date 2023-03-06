<template>
  <main>
    <SectionHero></SectionHero>
    <div class="main-container">
      <SectionProjects id="projects"></SectionProjects>
      <SectionAboutMe class="item" id="skills"></SectionAboutMe>
    </div>
  </main>
</template>

<style lang="scss" scoped>
@import "~assets/variables.scss";

.item {
  margin: 64px 0;
}
</style>
