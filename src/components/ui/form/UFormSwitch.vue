<template>
  <div class="switcher">
    <input v-model="isChecked" :id="`form-checkbox-${name}`" :disabled="disabled" class="switcher__input"
      type="checkbox" @change="handleChange" />
    <label :for="`form-checkbox-${name}`" class="switcher__label">
      <b class="switcher__label-title">{{ label }}</b>
      <p v-if="description" class="switcher__label-text">
        {{ description }}
      </p>
    </label>
  </div>
</template>

<script>

export default {
  name: 'UFormSwitch',

  emits: ['update:modelValue'],

  props: {
    label: {
      type: String,
      default: '',
    },

    description: {
      type: String,
      default: '',
    },

    name: {
      type: String,
      default: '',
    },

    modelValue: {
      type: [Boolean, Number],
      default: false,
    },

    disabled: Boolean,
  },

  data() {
    return {
      isChecked: this.modelValue
    }
  },

  methods: {
    handleChange() {
      this.$emit('update:modelValue', this.isChecked)
    },
  }
}
</script>

<style lang="scss" scoped>
.switcher {
  position: relative;
  display: inline-flex;
  align-items: center;

  &__input {
    position: absolute;
    width: 1px;
    height: 1px;
    overflow: hidden;
    clip: rect(0 0 0 0);
    top: 0;
    left: 0;
    opacity: 0;

    &:checked~.switcher__label::before {
      background-color: $accent-color;
    }

    &:focus-visible~.switcher__label::before {
      border-color: $accent-color;
      box-shadow: 0 0 0 $border-width-2x #cdced8;
    }

    &:checked~.switcher__label::after {
      transform: translateX(calc(100% + 2px));
    }

    &:disabled~.switcher__label {
      cursor: not-allowed;

      color: var(--text-color-label-disabled);

      &::before {
        cursor: not-allowed;
        background-color: var(--background-color-btn-disabled);
      }

      &::after {
        border-color: transparent;
      }

    }

    &:disabled:checked~.switcher__label {
      cursor: not-allowed;

      &::before {
        background-color: var(--background-color-btn-disabled);
        border-color: var(--background-color-btn-disabled);
      }

      &::after {
        border-color: var(--color-one-accent);
      }
    }

    &:not(:disabled)~.switcher__label:hover {
      color: var(--text-color);

      &::before {
        border-color: var(--color-one-accent);
      }
    }

  }


  &__label {
    padding-left: 45px;
    display: block;
    position: relative;
    color: var(--text-sub-color);
    cursor: pointer;
    user-select: none;

    &::before {
      content: "";
      position: absolute;
      cursor: pointer;
      top: 1px;
      left: 0;
      width: 40px;
      height: 19px;
      background-color: $input-border-color;
      border-radius: 100px;
      transition: background-color $transition-duration $transition-timing-function, border-color $transition-duration $transition-timing-function,
        box-shadow $transition-duration $transition-timing-function;

    }

    &::after {
      content: "";
      position: absolute;
      width: 19px;
      height: 19px;
      left: 0;
      top: 1px;
      background-color: white;
      border-radius: 100px;
      border: 1px solid #cdced8;
      transition: transform $transition-duration $transition-timing-function, border-color $transition-duration $transition-timing-function;
    }

    &-text {
      font-size: 16px;
      line-height: 19px;
      font-weight: 500;
    }

    &-text {
      margin-top: rem(10);
      font-size: rem(14);
      line-height: rem(19);
      letter-spacing: -0.0015em;
      color: #696977;
    }
  }
}
</style>
