<template>
  <ul>
    <li v-for="socialItem in socialList" v-bind:key="socialItem.link">
      <a :aria-label="socialItem.alt" :href="socialItem.link" :class="socialItem.class">
        <fa :icon="socialItem.icon"></fa>
      </a>
    </li>
  </ul>
</template>

<script>
  import { faGithub, faLinkedin, faDiscord } from "@fortawesome/free-brands-svg-icons";

  export default {
    data: function () {
      return {
        socialList: [{
            link: "https://github.com/peter-r-g",
            alt: "GitHub",
            icon: faGithub,
            class: "text-github"
          },
          {
            link: "https://www.linkedin.com/in/peter-r-g/",
            alt: "LinkedIn",
            icon: faLinkedin,
            class: "text-linkedin"
          },
          {
            link: "https://discord.com/users/162321704419459072",
            alt: "Discord",
            icon: faDiscord,
            class: "text-discord"
          }
        ]
      }
    }
  }
</script>

<style lang="scss" scoped>
@import "~assets/variables.scss";

ul {
  list-style: none;
  padding: 0;
  display: flex;
  width: auto;
  gap: 20px;
  margin: 0;
}

li {
  padding: 0;
  margin: 0;
  text-align: center;
  font-size: 16pt;
}

.text-github:hover {
  color: $text-github;
}

.text-linkedin:hover {
  color: $text-linkedin;
}


.text-discord:hover {
  color: $text-discord;
}
</style>
