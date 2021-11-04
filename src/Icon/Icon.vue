<template>
  <div class="icon" :style="iconStyle">
    <svg :viewBox="viewBox" xmlns="http://www.w3.org/2000/svg">
      <g :style="colorStyle">
        <component :is="name" />
      </g>
    </svg>
  </div>
</template>

<script>
import { computed } from 'vue'
import DeleteIcon from './assets/DeleteIcon'
import SearchIcon from './assets/SearchIcon'

export default {
  name: 'VIcon',
  components: {
    DeleteIcon,
    SearchIcon
  },
  props: {
    name: {
      type: String,
      default: () => null,
    },
    color: {
      type: String,
      default: 'currentColor',
    },
    colorHover: {
      type: String,
      default: 'currentColor',
    },
    size: {
      type: [Number, String],
      default: 16,
    },
    viewBox: {
      type: String,
      default: '0 0 16 16',
    },
    width: {
      type: [Number, String],
      default: null,
    },
    height: {
      type: [Number, String],
      default: null,
    },
  },
  setup (props) {
    const iconStyle = computed(() => {
      return {
        width: `${props.width || props.size}px`,
        height: `${props.height || props.size}px`,
        minWidth: `${props.width || props.size}px`,
        minHeight: `${props.height || props.size}px`,
      }
    })

    const colorStyle = computed(() => {
      return {
        '--color': props.color,
        '--color-hover': props.colorHover,
      }
    })

    return {
      iconStyle,
      colorStyle,
    }
  },
}
</script>
<style lang="css" scoped>
.icon g {
  fill: var(--color);
}

.icon {
  display: flex;

  & > svg {
    width: 100%;
  }
}
</style>
