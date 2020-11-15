<script>
export default {
  name: 'Footer',

  computed: {
    currentYear() {
      return new Date().getFullYear();
    },
  },
};
</script>

<template>
  <div class="footer-content">
    <div class="footer-copy">
      <a
        href="https://github.com/bradleyhop/"
        rel="noopener"
        target="_blank"
        class="footer-link"
      >
        Bradley Smith
      </a>
        &#169;{{ currentYear }}
      <a
        href="https://github.com/bradleyhop/d3-bar-chart"
        rel="noopener"
        target="_blank"
        class="footer-link"
      >
        [ Project GitHub ]
      </a>
    </div>
  </div>
</template>

<style scoped lang="scss">
.footer-content {
  height: 2.5rem;
  padding: 0.5rem;
  margin: 0;
  color: $text-gray;
  text-align: left;
}

.footer-copy {
  float: right;
  margin-right: 1rem;
  margin-bottom: 1rem;
  line-height: 1.2rem;
}

.footer-link {
  color: $text-gray;
  text-decoration: none;
}

.footer-link:hover {
  text-decoration: underline;
}

.footer-link:active {
  color: $text-gray;
}
</style>
