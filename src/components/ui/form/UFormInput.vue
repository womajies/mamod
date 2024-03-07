<template>
  <label class="input" :class="classes">
    <input :value="modelValue" :type="computedType" :name="name" @input="handleInput" @focus="handleFocus" @blur="handleBlur"
      class="input__value" :placeholder="placeholder" :disabled="disabled" />
    <span v-if="label" class="input__label">{{ label }}</span>
    <span v-if="error" class="input__error">
      <span class="input__error-hint">{{ error }}</span>
      <UIcon class="input__error-icon" name="error" />
    </span>
    <button
      v-if="isPasswordInput"
      class="input__password-btn"
      type="button"
      :title="isPasswordShown ? 'Скрыть' : 'Показать'"
      @click.stop="isPasswordShown = !isPasswordShown"
    >
      <UIcon :name="isPasswordShown ? 'eye-off' : 'eye-on'" />
    </button>
  </label>
</template>

<script>
import UIcon from '@/components/ui/UIcon.vue'

export default {
  name: "UFormInput",

  components: { UIcon },

  props: {
    label: {
      type: String,
      raquired: true,
    },
    type: {
      type: String,
      default: 'text',
    },
    placeholder: {
      type: String,
    },
    error: {
      type: String,
    },
    modelValue: {
      type: String,
      default: '',
    },
    disabled: Boolean,
    mask: String,
    name: String,
  },

  emits: ['update:modelValue'],

  data() {
    return {
      isErrorValidation: false,
      isSuccessValidation: false,
      isFocus: false,
      isPasswordShown: false,
    };
  },

  methods: {
    handleInput(evt) {
      this.$emit('update:modelValue', evt.target.value.trim())
    },
    handleFocus() {
      this.isFocus = true;
    },
    handleBlur() {
      this.isFocus = false
    },
  },

  computed: {
    classes() {
      return {
        'input--error': this.error,
        'input--valid': this.isSuccessValidation,
        'input--disabled': this.disabled,
        'input--fill': this.modelValue.length || this.placeholder,
        'input--focus': this.isFocus,
      }
    },

    computedType() {
      if (this.isPasswordInput) return this.isPasswordShown ? 'text' : 'password'
      else return this.type || 'text'
    },

    isPasswordInput() {
      return this.type === 'password'
    },

    isButtonShifted() {
      return this.isErrorValidation || (this.isSuccessValidation && this.displaySuccessValidation)
    },
  }
};
</script>

<style lang="scss" scoped>
.input {
  position: relative;
  z-index: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  max-width: rem(450);
  width: 100%;
  background-clip: padding-box;
  border-radius: $border-radius-input;

  &--fill {
    .input__label {
      bottom: 105%;
      transform: translateY(0%);
    }
  }

  &--error {
    .input__value {
      padding-right: rem(70);
      border-color: $error-color;

      @include on-breakpoint('md') {
        padding-right: rem(80);
      }
    }

    .input__password-btn {
      right: rem(50);
    }

    .input__error {
      &-icon {
        width: rem(18) !important;
        height: rem(18) !important;
        position: absolute;
        right: rem(10);
        top: 50%;
        transform: translateY(-50%);

        @include on-breakpoint('md') {
          width: rem(26) !important;
          height: rem(26) !important;
        }
      }

      &-hint {
        width: rem(150);
        max-width: max-content;
        position: absolute;
        z-index: 1;
        bottom: calc(100% - 0.3rem);
        right: 0;
        transform: translateX(10.5%);
        max-width: rem(100);
        padding: rem(4) rem(8);
        font-size: rem(12);
        line-height: 120%;
        text-align: center;
        word-wrap: break-word;
        outline: $border-width solid $error-color;
        background-color: $white-color;
        border-radius: $border-radius-input;

        @include on-breakpoint('md') {
          transform: translateX(calc(50% - 1.45rem));
          padding: rem(8) rem(10);
        }

        &::before {
          content: "";
          position: absolute;
          bottom: rem(-7);
          right: 23.5%;
          transform: translateX(50%);
          border-radius: 50%;
          border-top-width: rem(13);
          border-left-width: rem(13);
          border-right-width: rem(13);
          border-color: $accent-color transparent transparent transparent;

          @include on-breakpoint('md') {
            right: 50%;
          }
        }
      }
    }
  }

  &:not(.input--disabled).input--focus {
    .input__label {
      bottom: 105%;
      transform: translateY(0%);
    }
  }

  &--disabled {
    .input__value {
      background-color: #333333;
      color: #dadada;
    }

    .input__label {
      color: rgba(#dadada, 0.5);
    }
  }

  &__value {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    min-height: rem(40);
    padding: rem(10) rem(40) rem(10) rem(10);
    font-size: rem(14);
    line-height: rem(19);
    font-weight: 400;
    border-radius: $border-radius-input;
    color: $black-color;
    border: $border-width solid $input-border-color;
    transition: border-color $transition-timing-function $transition-duration;

    &::placeholder {
      font-size: rem(12);
      line-height: rem(14);
      font-weight: 400;
      color: $input-placeholder-color;
    }

    &:not(&[disabled]):hover {
      border: $border-width solid rgba($accent-color, 0.7);
    }

    &:not(&[disabled]):focus {
      border: $border-width solid rgba($accent-color, 0.7);

      &+.input__label {
        bottom: 105%;
        top: auto;
        transform: translateY(0%);
      }
    }
  }

  &__label {
    position: absolute;
    left: rem(10);
    bottom: 50%;
    transform: translateY(50%);
    font-size: rem(14);
    line-height: rem(16);
    font-weight: 400;
    color: $input-placeholder-color;
    cursor: text;
    transition: transform $transition-duration $transition-timing-function, bottom $transition-duration $transition-timing-function, font-size $transition-duration $transition-timing-function;
  }

  &__password-btn {
    display: flex;
    position: absolute;
    right: rem(10);
    color: #a4a4a6;
    transition: color $transition-duration $transition-timing-function;

    &:hover, &:focus {
      color: $accent-color;
    }
  }
}
</style>