<script setup lang="ts">
const { path } = useRoute();
const featureFlags = useFeatureFlags();
const links = useLinks();
const scrolled = ref(false);
const menuOpen = ref(false);

const { alwaysOpaque = false } = defineProps<{
  alwaysOpaque?: boolean;
}>();

function updateScrollPosition(): void {
  scrolled.value = window.scrollY > 0;
}

function toggleMenu(): void {
  menuOpen.value = !menuOpen.value;
}

onMounted(() => {
  if (alwaysOpaque) {
    return;
  }

  updateScrollPosition();

  window.addEventListener("scroll", updateScrollPosition, { passive: true });

  return () => {
    window.removeEventListener("scroll", updateScrollPosition);
  };
});
</script>

<template>
  <header
    :class="{
      'header--scrolled': scrolled || alwaysOpaque,
    }"
    class="header"
  >
    <MaxWidthContainer
      :class="{
        'header-inner--opened': menuOpen,
      }"
      class="header-inner"
    >
      <NuxtLink aria-label="back to home" href="/">
        <img
          alt="Redot logo"
          class="header-logo"
          src="~/assets/images/TopBarLogo.svg"
        >
      </NuxtLink>
      <button
        aria-label="mobile-menu"
        class="header-mobile-menu-btn"
        @click="toggleMenu"
      >
        <Icon :name="menuOpen ? 'close' : 'menu'" />
      </button>
      <div
        :class="{
          'header-navigation--opened': menuOpen,
          'header-navigation--minimal': featureFlags.minimal,
        }"
        class="header-navigation"
      >
        <div class="header-links">
          <NuxtLink
            key="documentation"
            :href="links.documentation"
            aria-label="documentation-link"
            class="header-link"
          >
            Documentation
          </NuxtLink>
          <NuxtLink
            key="news"
            :class="{
              'header-menu-link--active': path === '/news',
            }"
            aria-label="news-link"
            class="header-link"
            href="/news"
          >
            News
          </NuxtLink>
          <NuxtLink
            key="proposals"
            :href="links.proposals"
            aria-label="proposals-link"
            class="header-link"
          >
            Proposals
          </NuxtLink>
          <NuxtLink
            v-if="!featureFlags.minimal"
            key="assets"
            aria-label="assets-link"
            class="header-link"
            href="#"
          >
            Assets
          </NuxtLink>
        </div>

        <div
          :class="{
            'header-buttons--minimal': featureFlags.minimal,
          }"
          class="header-buttons"
        >
          <LinkButton :href="links.githubUrl" aria-label="contribute">
            Contribute
            <Icon name="code" />
          </LinkButton>
          <LinkButton :href="links.donation" aria-label="donate">
            Donate
            <Icon name="heart" />
          </LinkButton>
          <LinkButton
            v-show="scrolled"
            aria-label="download"
            href="/download"
            type="red"
          >
            Download
            <Icon name="arrow" />
          </LinkButton>
        </div>
      </div>
    </MaxWidthContainer>
  </header>
</template>

<style scoped lang="scss">
@use "@/assets/styles/mixins";

.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  background-color: #0000;
  transition: background-color 0.3s, backdrop-filter 0.3s,
    border-bottom-color 0.3s;
  font-size: 13px;
  border-bottom: 1px solid #fff0;
  padding-bottom: -1px;

  &--scrolled {
    background-color: #000000e6;
    backdrop-filter: blur(20px);
    border-bottom-color: #ffffff19;

    @include mixins.mobile-and-smaller {
      backdrop-filter: unset;
      background-color: #000;
    }
  }

  &-mobile-menu-btn {
    display: none;

    @include mixins.mobile-and-smaller {
      display: flex;
      align-items: center;
      padding: 8px;
      cursor: pointer;
      background-color: #000000e6;
      border: 1px solid #ffffff1a;
      border-radius: 8px;
    }
  }

  &-navigation {
    display: flex;
    justify-content: space-between;
    transition: opacity 0.3s, transform 0.3s;
    width: 100%;

    @include mixins.mobile-and-smaller {
      position: absolute;
      opacity: 0;
      top: 56px;
      left: 0;
      flex-direction: column;
      gap: 20px;
      align-items: start;
      height: 75vh;
      padding: 20px;
      border-bottom-left-radius: 20px;
      border-bottom-right-radius: 20px;
      border-bottom: 1px solid #ffffff1a;
      transform: translateY(-10px);
      pointer-events: none;
    }

    &--opened {
      opacity: 1;
      transform: translateY(0);
      pointer-events: auto;
      background-color: #000000e6;
      backdrop-filter: blur(20px);
    }

    &--minimal {
      height: unset;
    }
  }

  &-inner {
    display: flex;
    gap: 40px;
    height: 56px;
    align-items: center;

    @include mixins.mobile-and-smaller {
      gap: 0;
      flex-direction: row;
      align-items: center;
      justify-content: space-between;
    }

    &--opened {
      background-color: #000000e6;
    }
  }

  &-logo {
    width: 124px;
    height: 28px;
  }

  &-links {
    display: flex;
    gap: 20px;
    align-items: center;
    flex: 1;

    @include mixins.mobile-and-smaller {
      flex-direction: column;
      align-items: start;
    }
  }

  &-link {
    color: #fff;
    opacity: 60%;
    text-decoration: none;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    transition: opacity 0.1s;

    &:hover {
      opacity: 100%;
    }

    @include mixins.mobile-and-smaller {
      opacity: 100%;
    }
  }

  &-link-with-menu {
    position: relative;

    &:hover .header-menu {
      opacity: 1;
      pointer-events: auto;
    }

    @include mixins.mobile-and-smaller {
      display: block;
    }
  }

  &-menu {
    position: absolute;
    top: calc(100% + 10px);
    left: 50%;
    background-color: #000;
    border: 1px solid #191919;
    border-radius: 16px;
    padding: 10px;
    display: flex;
    opacity: 0;
    pointer-events: none;
    flex-direction: column;
    min-width: 200px;
    transform: translateX(-50%);
    transition: opacity 0.3s;

    @include mixins.mobile-and-smaller {
      position: unset;
      opacity: 1;
      transform: unset;
      border: unset;
      background-color: unset;
    }

    &::before {
      content: "";
      position: absolute;
      top: -20px;
      left: 0;
      right: 0;
      height: 40px;
      background-color: #00000001;
    }
  }

  &-menu-link {
    color: #999;
    padding: 10px;
    border-radius: 8px;
    text-decoration: none;
    transition: background-color 0.1s, color 0.1s;

    &:hover {
      color: #fff;
      background-color: #191919;
    }

    &--active {
      color: #fff;
      opacity: 1;
    }
  }

  &-buttons {
    display: flex;
    gap: 10px;

    @include mixins.mobile-and-smaller {
      justify-content: center;
      width: 100%;
    }

    &--minimal {
      justify-content: start;
    }
  }
}
</style>
