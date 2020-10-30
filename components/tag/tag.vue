<template>
  <transition :name="transitionName">
    <div :class="className">
      <span>
        <slot></slot>
      </span>
      <div
        v-if="closable"
        :class="`${prefix}__close`"
        @click="handleClose"
      >
        <Icon name="times" :scale="0.8"></Icon>
      </div>
    </div>
  </transition>
</template>

<script>
import Icon from '../icon'
import '../../icons/times'

const { prefix } = require('../../src/style/basis/variable')

export default {
  name: 'Tag',
  components: {
    Icon
  },
  props: {
    type: {
      default: 'default',
      validator(value) {
        return ['default', 'primary', 'success', 'error', 'warning'].includes(
          value
        )
      }
    },
    border: {
      type: Boolean,
      default: false
    },
    closable: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      prefix: `${prefix}-tag`,
      transitionName: `${prefix}-fade`
    }
  },
  computed: {
    className() {
      const { prefix, type, border } = this

      return {
        [prefix]: true,
        [`${prefix}--${type}`]: type !== 'default',
        [`${prefix}--border`]: border
      }
    }
  },
  methods: {
    handleClose() {
      if (this.closable) {
        this.$emit('on-close')
      }
    }
  }
}
</script>