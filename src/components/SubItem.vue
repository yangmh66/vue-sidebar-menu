<template>
  <div
    class="vsm-item"
    :class="[{'open-item' : show}, {'active-item' : active}, {'parent-active-item' : childActive}]"
  >
    <template v-if="isRouterLink">
      <router-link
        class="vsm-link"
        :class="item.class"
        :to="item.href"
        :disabled="item.disabled"
        :event="item.disabled ? '' : 'click'"
        v-bind="item.attributes"
        @click.native="clickEvent"
      >
        <i
          v-if="item.icon"
          class="vsm-icon"
          :class="item.icon"
        />
        <span
          v-if="item.badge"
          :style="[rtl ? (item.child ? {'margin-left' : '30px'} : '') : (item.child ? {'margin-right' : '30px'} : '')]"
          class="vsm-badge"
          :class="[item.badge.class ? item.badge.class : 'default-badge']"
          v-bind="item.badge.attributes"
        >{{ item.badge.text }}</span>
        <span class="vsm-title">{{ item.title }}</span>
        <i
          v-if="item.child"
          class="vsm-arrow"
          :class="{'open-arrow' : show}"
        />
      </router-link>
    </template>
    <template v-else>
      <a
        class="vsm-link"
        :class="item.class"
        :href="item.href ? item.href : '#'"
        :disabled="item.disabled"
        v-bind="item.attributes"
        @click="clickEvent"
      >
        <i
          v-if="item.icon"
          class="vsm-icon"
          :class="item.icon"
        />
        <span
          v-if="item.badge"
          :style="[rtl ? (item.child ? {'margin-left' : '30px'} : '') : (item.child ? {'margin-right' : '30px'} : '')]"
          class="vsm-badge"
          :class="[item.badge.class ? item.badge.class : 'default-badge']"
          v-bind="item.badge.attributes"
        >{{ item.badge.text }}</span>
        <span class="vsm-title">{{ item.title }}</span>
        <i
          v-if="item.child"
          class="vsm-arrow"
          :class="{'open-arrow' : show}"
        />
      </a>
    </template>
    <template v-if="item.child">
      <transition
        name="expand"
        @enter="expandEnter"
        @afterEnter="expandAfterEnter"
        @beforeLeave="expandBeforeLeave"
      >
        <div
          v-if="show"
          class="vsm-dropdown"
        >
          <div class="vsm-list">
            <item
              v-for="(subItem, index) in item.child"
              :key="index"
              :item="subItem"
            />
          </div>
        </div>
      </transition>
    </template>
  </div>
</template>

<script>
import Item from './Item.vue'
import { itemMixin, animationMixin } from '../mixin'

export default {
  components: {
    Item
  },
  mixins: [itemMixin, animationMixin],
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  beforeCreate () {
    this.$options.components.Item = require('./Item.vue').default
  }
}
</script>
