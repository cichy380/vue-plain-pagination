<template>
  <ul :class="paginationClasses.ul">
    <li v-if="paginationLabels.first"
        :class="`${paginationClasses.li} ${hasFirst ? paginationClasses.liDisable : ''}`">
      <button @click="first"
              :disabled="hasFirst"
              :class="`${paginationClasses.button} ${hasFirst ? paginationClasses.buttonDisable : ''}`"
              v-html="paginationLabels.first"></button>
    </li>
    <li v-if="paginationLabels.prev"
        :class="`${paginationClasses.li} ${hasFirst ? paginationClasses.liDisable : ''}`">
      <button @click="prev"
              :disabled="hasFirst"
              :class="`${paginationClasses.button} ${hasFirst ? paginationClasses.buttonDisable : ''}`"
              v-html="paginationLabels.prev"></button>
    </li>
    <li v-show="rangeFirstPage !== 1"
        :class="paginationClasses.li">
      <button @click="goto(1)"
              :class="paginationClasses.button">1</button>
    </li>
    <li v-show="rangeFirstPage === 3"
        :class="paginationClasses.li">
      <button @click="goto(2)"
              :class="paginationClasses.button">2</button>
    </li>
    <li v-show="rangeFirstPage !== 1 && rangeFirstPage !== 2 && rangeFirstPage !== 3"
        :class="`${paginationClasses.li} ${paginationClasses.liDisable}`">
      <span :class="`${paginationClasses.button} ${paginationClasses.buttonDisable}`">...</span>
    </li>
    <!-- range start -->
    <li v-for="page in range"
        :key="page"
        :class="`${paginationClasses.li} ${hasActive(page) ? paginationClasses.liActive : ''}`">
      <button @click="goto(page)"
              :class="`${paginationClasses.button} ${hasActive(page) ? paginationClasses.buttonActive : ''}`">{{ page }}</button>
    </li>
    <!-- range end -->
    <li v-show="rangeLastPage !== pageCount && rangeLastPage !== (pageCount - 1) && rangeLastPage !== (pageCount - 2)"
        :class="`${paginationClasses.li} ${paginationClasses.liDisable }`">
      <span :class="`${paginationClasses.button} ${paginationClasses.buttonDisable }`">...</span>
    </li>
    <li v-show="rangeLastPage === (pageCount - 2)"
        :class="paginationClasses.li">
      <button @click="goto(pageCount - 1)"
              :class="paginationClasses.button">{{ (pageCount - 1) }}</button>
    </li>
    <li v-if="rangeLastPage !== pageCount"
        :class="paginationClasses.li">
      <button @click="goto(pageCount)"
              :class="paginationClasses.button">{{ pageCount }}</button>
    </li>
    <li v-if="paginationLabels.next"
        :class="`${paginationClasses.li} ${hasLast ? paginationClasses.liDisable : ''}`">
      <button @click="next"
              :disabled="hasLast"
              :class="`${paginationClasses.button} ${hasLast ? paginationClasses.buttonDisable : ''}`"
              v-html="paginationLabels.next"></button>
    </li>
    <li v-if="paginationLabels.last"
        :class="`${paginationClasses.li} ${hasLast ? paginationClasses.liDisable : ''}`">
      <button @click="last"
              :disabled="hasLast"
              :class="`${paginationClasses.button} ${hasLast ? paginationClasses.buttonDisable : ''}`"
              v-html="paginationLabels.last"></button>
    </li>
  </ul>
</template>

<script>
  const rangeMax = 3;
  const defaultClasses = {
    ul: 'pagination',
    li: 'pagination-item',
    liActive: 'pagination-item--active',
    liDisable: 'pagination-item--disable',
    button: 'pagination-link',
    buttonActive: 'pagination-link--active',
    buttonDisable: 'pagination-link--disable'
  };
  const defaultLabels = {
    first: '&laquo;',
    prev: '&lsaquo;',
    next: '&rsaquo;',
    last: '&raquo;'
  };

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
        default: () => ({})
      },
      labels: {
        type: Object,
        required: false,
        default: () => ({})
      }
    },

    data() {
      return {
        paginationClasses: {
          ...defaultClasses,
          ...this.classes
        },
        paginationLabels: {
          ...defaultLabels,
          ...this.labels
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
