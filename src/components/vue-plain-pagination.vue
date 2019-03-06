<template>
  <ul :class="paginationClasses.ul">
    <li
      v-if="paginationLabels.first"
      :class="`${paginationClasses.li} ${hasFirst ? paginationClasses.liDisable : ''}`"
    >
      <button
        @click="first"
        :disabled="hasFirst"
        :class="`${paginationClasses.button} ${hasFirst ? paginationClasses.buttonDisable : ''}`"
        v-html="paginationLabels.first"
      ></button>
    </li>

    <li
      v-if="paginationLabels.prev"
      :class="`${paginationClasses.li} ${hasFirst ? paginationClasses.liDisable : ''}`"
    >
      <button
        @click="prev"
        :disabled="hasFirst"
        :class="`${paginationClasses.button} ${hasFirst ? paginationClasses.buttonDisable : ''}`"
        v-html="paginationLabels.prev"
      ></button>
    </li>

    <li
      v-for="page in items"
      :key="page.label"
      :class="`${paginationClasses.li} ${page.active ? paginationClasses.liActive : ''} ${page.disable ? paginationClasses.liDisable : ''}`"
    >
      <span
        v-if="page.disable"
        :class="`${paginationClasses.button} ${paginationClasses.buttonDisable}`"
      >
        ...
      </span>
      <button
        v-else
        @click="goto(page.label)"
        :class="`${paginationClasses.button} ${page.active ? paginationClasses.buttonActive : ''}`"
      >
        {{ page.label }}
      </button>
    </li>

    <li
      v-if="paginationLabels.next"
      :class="`${paginationClasses.li} ${hasLast ? paginationClasses.liDisable : ''}`"
    >
      <button
        @click="next"
        :disabled="hasLast"
        :class="`${paginationClasses.button} ${hasLast ? paginationClasses.buttonDisable : ''}`"
        v-html="paginationLabels.next"
      ></button>
    </li>

    <li
      v-if="paginationLabels.last"
      :class="`${paginationClasses.li} ${hasLast ? paginationClasses.liDisable : ''}`"
    >
      <button
        @click="last"
        :disabled="hasLast"
        :class="`${paginationClasses.button} ${hasLast ? paginationClasses.buttonDisable : ''}`"
        v-html="paginationLabels.last"
      ></button>
    </li>
  </ul>
</template>

<script>
  const defaultClasses = {
    ul: 'pagination',
    li: 'pagination-item',
    liActive: 'pagination-item--active',
    liDisable: 'pagination-item--disable',
    button: 'pagination-link',
    buttonActive: 'pagination-link--active',
    buttonDisable: 'pagination-link--disable'
  }

  const defaultLabels = {
    first: '&laquo;',
    prev: '&lsaquo;',
    next: '&rsaquo;',
    last: '&raquo;'
  }

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
        this.$emit('input', this.pageCount)
      }
    },

    computed: {
      items() {
        let valPrev = this.value > 1 ? (this.value - 1) : 1 // for easier navigation - gives one previous page
        let valNext = this.value < this.pageCount ? (this.value + 1) : this.pageCount // one next page
        let extraPrev = valPrev === 3 ? 2 : null
        let extraNext = valNext === (this.pageCount - 2) ? (this.pageCount - 1) : null
        let dotsBefore = valPrev > 3 ? 2 : null
        let dotsAfter = valNext < (this.pageCount - 2) ? (this.pageCount - 1) : null

        let output = []

        for (let i = 1; i <= this.pageCount; i += 1) {
          if ([1, this.pageCount, this.value, valPrev, valNext, extraPrev, extraNext, dotsBefore, dotsAfter].includes(i)) {
            output.push({
              label: i,
              active: this.value === i,
              disable: [dotsBefore, dotsAfter].includes(i)
            })
          }
        }

        return output
      },

      hasFirst() {
        return (this.value === 1)
      },

      hasLast() {
        return (this.value === this.pageCount)
      },
    },

    watch: {
      value: function () {
        this.$emit('change')
      }
    },

    methods: {
      first() {
        if (!this.hasFirst) {
          this.$emit('input', 1)
        }
      },

      prev() {
        if (!this.hasFirst) {
          this.$emit('input', (this.value - 1))
        }
      },

      goto(page) {
        this.$emit('input', page)
      },

      next() {
        if (!this.hasLast) {
          this.$emit('input', (this.value + 1))
        }
      },

      last() {
        if (!this.hasLast) {
          this.$emit('input', this.pageCount)
        }
      },
    }
  }
</script>
