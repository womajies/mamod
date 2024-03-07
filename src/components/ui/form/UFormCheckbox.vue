<template>
  <label class="checkbox" :class="classes">
    <input v-model="isChecked" type="checkbox" class="checkbox__input" :name="name" @change="handleChange" />
    <div class="checkbox__custom"></div>
    <div class="checkbox__caption">
      <slot>{{ label }}</slot>
    </div>
  </label>
</template>

<script>
export default {
  name: 'UFormCheckbox',

  emits: ['update:modelValue'],

  props: {
    modelValue: Boolean,
    name: String,
    error: [Boolean, String],
    label: {
      type: String,
    },
  },

  data () {
    return {
      isChecked: this.modelValue
    }
  },

  computed: {
    classes() {
      return {
        'checkbox--error': this.error
      }
    },
  },

  methods: {
    handleChange() {
      this.$emit('update:modelValue', this.isChecked)
    },
  },
};
</script>

<style lang="scss">
.checkbox {
  position: relative;
  z-index: 0;
  max-width: rem(542);
  width: 100%;
  display: flex;
  align-items: flex-start;
  cursor: pointer;
  user-select: none;

  &--error {
    .checkbox__custom {
      &::before {
        background-color: $error-color;
      }
    }
  }

  &__input {
    position: absolute;
    width: 1px;
    height: 1px;
    margin: -1px;
    border: 0;
    padding: 0;
    white-space: nowrap;
    -webkit-clip-path: inset(100%);
    clip-path: inset(100%);
    clip: rect(0 0 0 0);
    overflow: hidden;
  }

  &__input:checked+.checkbox__custom {
    background-color: $accent-color;

    &::before {
      display: block;
    }
  }

  &__custom {
    position: relative;
    flex-shrink: 0;
    width: rem(19);
    height: rem(17);
    margin-left: rem(1);
    background-color: $input-border-color;
    border-radius: $border-radius;
    transition: background-color $transition-duration $transition-timing-function;

    &::before {
      content: "";
      display: none;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: rem(9);
      height: rem(7);
      background-image: url("@/assets/images/check.svg");
      background-repeat: no-repeat;
      background-position: center;
      background-size: cover;
    }
  }

  &__caption {
    margin-left: rem(14);
    font-size: rem(14);
    line-height: rem(19);
    letter-spacing: -0.0015em;
    font-weight: 400;
    text-align: left;
    color: $black-color;

    a {
      color: $accent-color;
      transition: color $transition-duration $transition-timing-function;

      &:hover, &:focus {
        text-decoration: underline;
      }
    }
  }
}
</style>
