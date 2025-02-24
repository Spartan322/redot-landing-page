<script setup lang="ts">
const featureFlags = useFeatureFlags();
const links = useLinks();
</script>

<template>
  <footer class="footer">
    <MaxWidthContainer class="footer-inner">
      <div class="footer-socials">
        <img
          alt="Redot logo"
          class="footer-logo"
          src="~/assets/images/TopBarLogo.svg"
        >
        <div class="footer-spacer" />
        <NuxtLink :href="links.githubUrl" aria-label="github">
          <img
            alt="GitHub"
            class="footer-social"
            src="~/assets/images/social-github.svg"
          >
        </NuxtLink>
        <NuxtLink :href="links.discordUrl" aria-label="discord">
          <img
            alt="Discord"
            class="footer-social"
            src="~/assets/images/social-discord.svg"
          >
        </NuxtLink>
        <NuxtLink :href="links.twitterUrl" aria-label="twitter">
          <img
            alt="Twitter"
            class="footer-social"
            src="~/assets/images/social-twitter.svg"
          >
        </NuxtLink>
        <NuxtLink :href="links.redditUrl" aria-label="reddit">
          <img
            alt="Reddit"
            class="footer-social"
            src="~/assets/images/social-reddit.svg"
          >
        </NuxtLink>
      </div>

      <div class="footer-columns">
        <div class="footer-column">
          <SectionTitle variant="h2" small>Redot Engine</SectionTitle>
          <NuxtLink aria-label="donate" class="footer-link" href="/donate">
            Donate
          </NuxtLink>
          <NuxtLink
            :href="links.releasePage"
            aria-label="download"
            class="footer-link"
          >
            Download
          </NuxtLink>
          <NuxtLink
            :href="links.documentation"
            aria-label="documentation"
            class="footer-link"
          >
            Documentation
          </NuxtLink>
          <NuxtLink
            :href="links.githubUrl"
            aria-label="source code"
            class="footer-link"
          >
            Source Code
          </NuxtLink>
        </div>

        <div class="footer-column">
          <SectionTitle variant="h2" small>Project</SectionTitle>

          <NuxtLink aria-label="news" class="footer-link" href="/news">
            News
          </NuxtLink>
          <NuxtLink
            :href="links.contribute"
            aria-label="contribution guidelines"
            class="footer-link"
          >
            Contribution Guidelines
          </NuxtLink>
          <NuxtLink v-if="!featureFlags.minimal" class="footer-link" href="#">
            Teams
          </NuxtLink>
          <NuxtLink v-if="!featureFlags.minimal" class="footer-link" href="#">
            Communities
          </NuxtLink>
        </div>

        <div class="footer-column">
          <SectionTitle variant="h2" small>Resources</SectionTitle>

          <NuxtLink v-if="!featureFlags.minimal" class="footer-link" href="#">
            Assets
          </NuxtLink>
          <NuxtLink
            :href="links.pressKit"
            aria-label="press kit"
            class="footer-link"
          >
            Press Kit
          </NuxtLink>
          <NuxtLink aria-label="contact" class="footer-link" href="/contact">
            Contact
          </NuxtLink>
          <NuxtLink
            aria-label="community"
            class="footer-link"
            href="/community"
          >
            Community
          </NuxtLink>
          <NuxtLink aria-label="license" class="footer-link" href="/license">
            License
          </NuxtLink>
        </div>
      </div>

      <SectionDescription class="footer-copyright">
        © 2024-present by the Redot community. Website
        <NuxtLink
          :href="links.websiteGithubUrl"
          aria-label="website source"
          class="source-link"
        >
          source code on Github
        </NuxtLink>
      </SectionDescription>
    </MaxWidthContainer>
  </footer>
</template>

<style scoped lang="scss">
@use "@/assets/styles/mixins";

.footer {
  border-top: 1px solid #191919;
  padding: 40px 0;

  &-inner {
    display: flex;
    flex-direction: column;
    gap: 30px;
  }

  &-socials {
    display: flex;
    align-items: center;
    gap: 10px;
    flex-wrap: wrap;

    @include mixins.mobile-and-smaller {
      justify-content: center;
    }
  }

  &-logo {
    width: 178px;
    height: 40px;
  }

  &-spacer {
    flex: 1;

    @include mixins.mobile-and-smaller {
      flex: none;
      width: 100%;
    }
  }

  &-social {
    width: 32px;
    height: 32px;
  }

  &-columns {
    display: flex;
    gap: 20px;

    @include mixins.mobile-and-smaller {
      flex-direction: column;
      gap: 20px;
    }
  }

  &-column {
    display: flex;
    flex-direction: column;
    gap: 10px;
    padding: 10px;
  }

  &-link {
    color: #fff;
    font-size: 16px;
    text-decoration: none;
    opacity: 60%;
    transition: opacity 0.1s;

    &:hover {
      opacity: 100%;
    }
  }

  &-copyright {
    @include mixins.mobile-and-smaller {
      padding: 0 10px;
    }

    :deep(a) {
      color: #3388ff;
    }

    :deep(a:hover) {
      text-decoration: underline;
    }
  }
}
</style>
