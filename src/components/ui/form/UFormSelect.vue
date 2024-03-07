<template>
  <div class="select" :class="classes">
    <button type="button" class="select__header" :style="options.length === 0 ? { cursor: 'not-allowed' } : ''"
      :disabled="options.length === 0" @focus="handleFocus" @blur="handleBlur" @click="toggleOptions">
      <div class="selected__option">
        {{ selectedOptionName || 'Должность' }}
      </div>
      <div class="select__label">
        Должность
      </div>
      <div class="select__icon">
        <img :class="{ 'rotate-180': showOptions }" src="@/assets/images/arrow-down.svg" alt="Arrow down">
      </div>
    </button>
    <transition name="fade">
      <div class="select__options" v-show="showOptions">
        <button v-for="option in options" :key="option.value" class="select__option"
          :class="{ 'select__option--active': option.value === selectedOptionType }" type="button"
          :disabled="option.value === selectedOptionType" @click="selectOption(option.value)">
          {{ option.name }}
        </button>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "UFormSelect",

  props: {
    options: {
      type: Array,
      required: true,
    },
    modelValue: {
      type: [String, Number],
      default: '',
    },
    error: {
      type: String,
    },
  },

  emits: ['update:modelValue', 'input'],

  data() {
    return {
      showOptions: false,
      isFocus: false,
    };
  },

  mounted() {
    document.addEventListener('keydown', this.handleKeyDown);
    document.addEventListener('click', this.handleClickOutside);
  },

  beforeUnmount() {
    document.removeEventListener('keydown', this.handleKeyDown);
    document.removeEventListener('click', this.handleClickOutside);
  },

  computed: {
    selectedOptionName() {
      let option;
      for (let i = 0; i < this.options.length; i++) {
        if (this.options[i].value === this.modelValue) {
          option = this.options[i].name;
        }
        continue;
      }
      return option;
    },

    selectedOptionType() {
      return this.modelValue
    },

    classes() {
      return {
        'select--focus': this.isFocus,
        'select--error': this.error,
        'select--placeholder': !this.selectedOptionName,
      }
    },
  },

  methods: {
    handleKeyDown(e) {
      if (e.keyCode === 27) this.showOptions = false;
    },

    handleClickOutside(event) {
      if (!this.$el.contains(event.target)) {
        this.showOptions = false;
      }
    },

    toggleOptions() {
      this.showOptions = !this.showOptions;
    },

    selectOption(option) {
      this.$emit('update:modelValue', option);
      this.$emit('input');
      this.showOptions = false;
    },

    handleFocus() {
      this.isFocus = true;
    },

    handleBlur() {
      this.isFocus = false
    },
  },
};
</script>

<style lang="scss">
.select {
  position: relative;
  display: inline-block;
  max-width: rem(450);
  width: 100%;
  background-color: $white-color;
  border-radius: $border-radius-input;
  border: $border-width solid $input-border-color;
  transition: border-color $transition-timing-function $transition-duration;

  &:hover {
    border: $border-width solid rgba($accent-color, 0.7);
  }

  &.select--focus {
    border: $border-width solid rgba($accent-color, 0.7);
  }

  &.select--error {
    border: $border-width solid $error-color;
  }

  &.select--placeholder {
    .selected__option {
      font-size: rem(14);
      line-height: rem(19);
      color: $input-placeholder-color;
    }

    .select__label {
      opacity: 0;
      visibility: hidden;
      bottom: 0;
    }
  }

  &__header {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    min-height: rem(40);
    padding: rem(10);
    cursor: pointer;
  }

  &__label {
    position: absolute;
    bottom: 110%;
    opacity: 1;
    visibility: visible;
    transform: translateY(0%);
    font-size: rem(14);
    line-height: rem(19);
    letter-spacing: -0.0015em;
    color: #9292A0;
    color: $input-placeholder-color;
    transition: bottom $transition-duration $transition-timing-function, opacity $transition-duration $transition-timing-function;
  }

  .selected__option {
    font-size: rem(14);
    line-height: rem(19);
    letter-spacing: -0.0015em;
    color: #9292A0;
    color: $black-color;

    &::first-letter {
      text-transform: uppercase;
    }
  }

  &__options {
    position: absolute;
    z-index: 10;
    top: calc(100% + 9px);
    left: 0;
    right: 0;
    display: flex;
    flex-direction: column;
    background-color: $white-color;
    outline: $border-width solid $accent-color;
    border-radius: $border-radius-input;
  }

  &__option {
    width: 100%;
    min-height: rem(40);
    padding: rem(10);
    font-size: rem(14);
    line-height: rem(19);
    text-align: left;
    border: $border-width solid $accent-color;
    transition: border-color $transition-duration $transition-timing-function;

    &:hover:not(.select__option--active),
    &:focus:not(.select__option--active) {
      background-color: rgba($btn-color, 0.08);
    }

    &:first-child {
      border-top-left-radius: $border-radius-input;
      border-top-right-radius: $border-radius-input;
    }

    &:last-child {
      border-bottom-left-radius: $border-radius-input;
      border-bottom-right-radius: $border-radius-input;
    }

    &::first-letter {
      text-transform: uppercase;
    }

    &--active {
      color: $input-placeholder-color;
      background-color: rgba($btn-color, 0.2);
      cursor: not-allowed;
    }
  }

  &__icon {
    width: rem(9);
    height: rem(5);
    margin-left: rem(10);

    img {
      transition: transform $transition-duration $transition-timing-function;
    }
  }

  .rotate-180 {
    transform: rotate(180deg);
  }
}
</style>