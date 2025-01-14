<template>
  <div class="f-tab">
    <FCard>
      <FCardTitle
        v-touch-swipe.mouse="userSwiped"
        ref="tabHeader"
        class="f-tab__header"
      >
        <FButtonGroup
          ref="btnGroup"
          :tab="!fill"
          :options="options"
          :default.sync="selected"
          v-bind="$attrs"
          @change="setSelected"
        />

        <div v-if="$slots['header-addon']" class="f-tab__header__addon">
          <slot name="header-addon" />
        </div>
      </FCardTitle>
      <FSeparator v-if="!noSeparator" />
      <FCardBody
        v-for="(item, index) in options"
        :key="index"
        :data-value="item.value"
      >
        <template v-if="item.value === selected">
          <slot :name="`content-${item.value}`" />
        </template>
      </FCardBody>
    </FCard>
  </div>
</template>

<script>
import { FCard, FCardBody, FCardTitle } from '../FCard'
import { FSeparator } from '../FSeparator'
import { FButtonGroup } from '../FButton'

export default {
  name: 'f-tab',

  components: {
    FCard,
    FCardBody,
    FSeparator,
    FButtonGroup,
    FCardTitle
  },

  props: {
    fill: Boolean,
    options: Array,
    noSeparator: Boolean,
    value: {
      type: [Number, String],
      default: 1
    }
  },

  data: () => ({ selected: null }),

  computed: {
    headerSize() {
      return this.$refs.tabHeader && this.$refs.tabHeader.$el
        ? this.$refs.tabHeader.$el.clientWidth
        : 0
    },
    btnGroupSize() {
      return this.$refs.btnGroup && this.$refs.btnGroup.$el
        ? this.$refs.btnGroup.$el.clientWidth
        : 0
    }
  },

  created() {
    this.selected = this.value || 1
  },

  methods: {
    setSelected(id) {
      this.selected = id
      this.$emit('change', id)
      this.$emit('input', id)
    },
    drag(e) {
      return e
    },
    stopDrag(e) {
      return e
    },
    userSwiped(e) {
      return e
    }
  }
}
</script>

<style lang="scss" scoped>
.f-tab {
  &__header {
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    overflow: auto;
    margin-bottom: 0px;
  }
}
</style>
