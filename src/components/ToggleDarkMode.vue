<template>
  <button
    @click="handleClick"
    aria-label="Toggle Darkmode"
    title="Toggle Darkmode"
  >
    <slot :dark="isDarkMode" />
  </button>
</template>

<script>
const DARK_MODE = "dark-mode";

export default {
  data() {
    return {
      isDarkMode: false,
    };
  },

  methods: {
    handleClick() {
      const hasDarkMode = document.documentElement.classList.contains(
        DARK_MODE
      );

      // Toggle dark mode on click.
      return this.toggleDarkMode(!hasDarkMode);
    },

    toggleDarkMode(shouldBeDark) {
      document.documentElement.classList.toggle(DARK_MODE, shouldBeDark);

      this.isDarkMode = shouldBeDark;

      this.writeToStorage(shouldBeDark);

      return shouldBeDark;
    },

    detectPrefered() {
      return window.matchMedia("(prefers-color-scheme: dark)").matches;
    },

    hasInStorage() {
      const check = localStorage.getItem(DARK_MODE);

      return check !== null;
    },

    writeToStorage(prefersDark) {
      localStorage.setItem(DARK_MODE, prefersDark ? "true" : "false");
    },

    getFromStorage() {
      return localStorage.getItem(DARK_MODE) === "true" ? true : false;
    },
  },

  mounted() {
    if (this.hasInStorage()) {
      this.toggleDarkMode(this.getFromStorage());
    } else if (process.isClient && window.matchMedia) {
      this.toggleDarkMode(this.detectPrefered());
    }
  },
};
</script>
