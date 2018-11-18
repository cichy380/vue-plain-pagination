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
    <li v-show="startPage !== 1"
        :class="classes.li">
      <button @click="goto(1)"
              :class="classes.button">1</button>
    </li>
    <li v-show="startPage === 3"
        :class="classes.li">
      <button @click="goto(2)"
              :class="classes.button">2</button>
    </li>
    <li v-show="startPage !== 1 && startPage !== 2 && startPage !== 3"
        :class="`${classes.li} ${classes.liDisable}`">
      <span :class="`${classes.button} ${classes.buttonDisable}`">...</span>
    </li>
    <li v-for="page in pages"
        :key="page"
        :class="`${classes.li} ${hasActive(page) ? classes.liActive : ''}`">
      <button @click="goto(page)"
              :class="`${classes.button} ${hasActive(page) ? classes.buttonActive : ''}`">{{ page }}</button>
    </li>
    <li v-show="endPage !== pageCount && endPage !== (pageCount - 1) && endPage !== (pageCount - 2)"
        :class="`${classes.li} ${classes.liDisable }`">
      <span :class="`${classes.button} ${classes.buttonDisable }`">...</span>
    </li>
    <li v-show="endPage === (pageCount - 2)"
        :class="classes.li">
      <button @click="goto(pageCount - 1)"
              :class="classes.button">{{ (pageCount - 1) }}</button>
    </li>
    <li v-if="endPage !== pageCount"
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
  const maxVisibleButtons = 3;

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

    computed: {
      startPage() {
        if (this.value === 1) {
          return 1;
        }

        if (this.value === this.pageCount) {
          return this.pageCount - maxVisibleButtons + 1;
        }

        return (this.value - 1);
      },

      endPage() {
        return Math.min(this.startPage + maxVisibleButtons - 1, this.pageCount);
      },

      pages() {
        let range = [];
        for (let page = this.startPage; page <= this.endPage; page+= 1) {
          range.push(page);
        }
        return range;
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
