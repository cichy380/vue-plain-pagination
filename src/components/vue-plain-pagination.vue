<template>
  <ul :class="classes.ul">
    <li :class="`${classes.li} ${hasFirst ? classes.liDisable : ''}`">
      <button @click="first"
              :disabled="hasFirst"
              :class="`${classes.button} ${hasFirst ? classes.buttonDisable : ''}`">&laquo;</button>
    </li>
    <li :class="`${classes.li} ${hasFirst ? classes.liDisable : ''}`">
      <button @click="prev"
              :disabled="hasFirst"
              :class="`${classes.button} ${hasFirst ? classes.buttonDisable : ''}`">&lsaquo;</button>
    </li>
    <li v-show="rangeFirstPage !== 1"
        :class="classes.li">
      <button @click="goto(1)"
              :class="classes.button">1</button>
    </li>
    <li v-show="rangeFirstPage === 3"
        :class="classes.li">
      <button @click="goto(2)"
              :class="classes.button">2</button>
    </li>
    <li v-show="rangeFirstPage !== 1 && rangeFirstPage !== 2 && rangeFirstPage !== 3"
        :class="`${classes.li} ${classes.liDisable}`">
      <span :class="`${classes.button} ${classes.buttonDisable}`">...</span>
    </li>
    <!-- range start -->
    <li v-for="page in range"
        :key="page"
        :class="`${classes.li} ${hasActive(page) ? classes.liActive : ''}`">
      <button @click="goto(page)"
              :class="`${classes.button} ${hasActive(page) ? classes.buttonActive : ''}`">{{ page }}</button>
    </li>
    <!-- range end -->
    <li v-show="rangeLastPage !== pageCount && rangeLastPage !== (pageCount - 1) && rangeLastPage !== (pageCount - 2)"
        :class="`${classes.li} ${classes.liDisable }`">
      <span :class="`${classes.button} ${classes.buttonDisable }`">...</span>
    </li>
    <li v-show="rangeLastPage === (pageCount - 2)"
        :class="classes.li">
      <button @click="goto(pageCount - 1)"
              :class="classes.button">{{ (pageCount - 1) }}</button>
    </li>
    <li v-if="rangeLastPage !== pageCount"
        :class="classes.li">
      <button @click="goto(pageCount)"
              :class="classes.button">{{ pageCount }}</button>
    </li>
    <li :class="`${classes.li} ${hasLast ? classes.liDisable : ''}`">
      <button @click="next"
              :disabled="hasLast"
              :class="`${classes.button} ${hasLast ? classes.buttonDisable : ''}`">&rsaquo;</button>
    </li>
    <li :class="`${classes.li} ${hasLast ? classes.liDisable : ''}`">
      <button @click="last"
              :disabled="hasLast"
              :class="`${classes.button} ${hasLast ? classes.buttonDisable : ''}`">&raquo;</button>
    </li>
  </ul>
</template>

<script>
  const rangeMax = 3;

  export default {
    props: {
      value: {  // current page
        type: Number,
        required: true
      },
      pageCount: { // page numbers
        type: Number,
        required: true
      },
      classes: {
        type: Object,
        required: false,
        default() {
          return {
            ul: 'pagination',
            li: 'pagination-item',
            liActive: 'pagination-item--active',
            liDisable: 'pagination-item--disable',
            button: 'pagination-link',
            buttonActive: 'pagination-link--active',
            buttonDisable: 'pagination-link--disable'
          }
        }
      }
    },

    mounted() {
      if (this.value > this.pageCount) {
        this.$emit('input', this.pageCount);
      }
    },

    computed: {
      rangeFirstPage() {
        if (this.value === 1) {
          return 1;
        }

        if (this.value === this.pageCount) {
          if ((this.pageCount - rangeMax) < 0) {
            return 1;
          }
          else {
            return this.pageCount - rangeMax + 1;
          }
        }

        return (this.value - 1);
      },

      rangeLastPage() {
        return Math.min(this.rangeFirstPage + rangeMax - 1, this.pageCount);
      },

      range() {
        let rangeList = [];
        for (let page = this.rangeFirstPage; page <= this.rangeLastPage; page+= 1) {
            rangeList.push(page);
        }
        return rangeList;
      },

      hasFirst() {
        return (this.value === 1);
      },

      hasLast() {
        return (this.value === this.pageCount);
      },
    },

    methods: {
      first() {
        if (!this.hasFirst) {
          this.$emit('input', 1);
        }
      },

      prev() {
        if (!this.hasFirst) {
          this.$emit('input', (this.value - 1));
        }
      },

      goto(page) {
        this.$emit('input', page);
      },

      next() {
        if (!this.hasLast) {
          this.$emit('input', (this.value + 1));
        }
      },

      last() {
        if (!this.hasLast) {
          this.$emit('input', this.pageCount);
        }
      },

      hasActive(page) {
        return (page === this.value);
      },
    }
  }
</script>
