<template>
  <svg
    class="loader animate-spin"
    xmlns="http://www.w3.org/2000/svg"
    fill="none"
    viewBox="0 0 24 24"
    v-bind="$attrs"
  >
    <circle
      class="opacity-25"
      cx="12"
      cy="12"
      r="10"
      stroke="currentColor"
      stroke-width="4"
    />
    <path
      class="opacity-75"
      fill="currentColor"
      d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
    />
  </svg>
</template>

<script>
export default {
  name: "ULoader",
};
</script>

<style lang="scss" scoped>
@-webkit-keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
.loader {
  position: absolute;
  top: 50%;
  left: 50%;
  margin: -8px -8px;
}
.animate-spin {
  width: 14px;
  -webkit-animation: spin 1s linear infinite;
  animation: spin 1s linear infinite;

  .opacity-25 {
    opacity: 0.25;
  }

  .opacity-75 {
    opacity: 0.75;
  }
}
</style>
