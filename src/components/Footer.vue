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
        href="https://github.com/bradleyhop/d3-scatter-plot"
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
  color: $text-gray;
  height: 2.5rem;
  margin: 0;
  padding: 0.5rem;
  text-align: left;
}

.footer-copy {
  float: right;
  line-height: 1.2rem;
  margin-bottom: 1rem;
  margin-right: 1rem;
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
