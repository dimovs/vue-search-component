<template>
  <div class="search__container" :class="{'search__container--active': modelValue.length > 0}">
    <input
        ref="inputElement"
        class="search"
        type="text"
        autocomplete="off"
        autocapitalize="off"
        autocorrect="off"
        :autofocus="hasAutoFocus"
        spellcheck="false"
        :placeholder="placeholder"
        :value="modelValue"
        :maxlength="maxlength"
        aria-label="Поиск"
        @input="$emit('update:modelValue', $event.target.value)"
        @keydown.enter.prevent="activeButton.focus()"
    >
    <button
        ref="activeButton"
        @click="handleClick"
        class="search__btn"
        type="button"
        :data-type="btnType"
        :aria-label="btnType === 'search' ? 'Найти' : 'Удалить'"
    >
      <v-icon
          v-if="btnType === 'search'"
          view-box="0 0 24 24"
          name="SearchIcon"
          size="24"
          class="icon"
          color="#939A9F"
          color-hover="#939A9F"
      />
      <v-icon
          v-if="btnType === 'delete'"
          name="DeleteIcon"
          size="24"
          class="icon"
          color="#333F48"
          color-hover="#333F48"
      />
    </button>
  </div>
</template>

<script>
import { computed, ref } from 'vue'

export default {
  name: 'Search',
  props: {
    modelValue: {
      type: String,
      default: '',
    },
    placeholder: {
      type: String,
      default: 'Поиск',
    },
    hasAutoFocus: {
      type: Boolean,
      default: false,
    },
    maxlength: {
      type: Number,
      default: 40,
    },
  },
  emits: {
    'update:modelValue': null,
  },
  setup (props, { emit }) {
    const inputElement = ref(null)
    const activeButton = ref(null)

    const btnType = computed(() => {
      return props.modelValue.length ? 'delete' : 'search'
    })

    function handleClick (e) {
      const type = e.currentTarget.dataset.type

      if (type === 'delete') {
        emit('update:modelValue', '')
      }

      if (inputElement.value) {
        inputElement.value.focus()
      }
    }

    return {
      handleClick,
      btnType,
      inputElement,
      activeButton,
    }
  },
}
</script>

<style lang="css" scoped>
.search__container {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  width: 100%;
  border-radius: 12px;
}

.search {
  flex: auto;
  outline: none;
  border: none;
  padding: 12px 0 12px 16px;
  background-color: transparent;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  letter-spacing: -0.02em;
}

.search__btn {
  padding: 0 16px;
  border: none;
  outline: none;
  background: none;
  line-height: 0;
  cursor: pointer;
}

.icon {
  display: inline-block;
}
</style>
