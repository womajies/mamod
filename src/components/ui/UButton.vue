<template>
  <component :is="to ? 'router-link' : href ? 'a' : 'button'" :class="classes" :disabled="disabled || loading"
    :style="styles" :to="!disabled ? to : null" :href="!disabled ? href : null" :target="href ? '_blank' : null"
    :type="type" @click="noClickEvent ? null : $emit('click', $event)">
    <slot name="loader">
      <ULoader v-if="loading" />
    </slot>

    <div :class="{ invisible: loading }" class="relative flex items-center justify-center">
      <slot name="prepend">
        <UIcon v-if="icon" :class="[String(label) || $slots.default || iconPadding === true ? 'mr-2' : iconPadding]"
          :clickable="!disabled && !String(label)" :name="icon" :section="iconSection" :size="iconSize ? iconSize : null"
          :small="smallIcon" class="flex-shrink-0" />
      </slot>
      <div :class="{ 'mr-5': appendIcon || $slots.append }" class="flex items-center justify-center"
        style="line-height: normal;">
        <slot>
          {{ label }}
        </slot>
      </div>
      <slot name="append">
        <UIcon v-if="appendIcon" :clickable="!disabled && !String(label)" :name="appendIcon" :section="appendIconSection"
          :size="appendIconSize" class="flex-shrink-0 ml-auto" />
      </slot>
    </div>
  </component>
</template>
<script>
import ULoader from "@/components/ui/ULoader";
import UIcon from "@/components/ui/UIcon";

export default {
  name: "UButton",

  components: { ULoader, UIcon },

  props: {
    label: {
      type: [Number, String],
      default: "",
    },
    variant: {
      type: String,
      default: "default",
      validator: (v) => !v || ["default", "primary", "secondary"].includes(v),
    },
    size: {
      type: String,
      default: '',
      validator: (v) => !v || ['sm'].includes(v),
    },
    padding: {
      type: String,
      default: null,
    },

    icon: {
      type: String,
      default: '',
    },
    iconSection: {
      type: String,
      default: '',
    },
    iconSize: {
      type: String,
    },
    iconPadding: {
      type: [Boolean, String],
      default: null,
    },

    noClickEvent: Boolean,
    smallIcon: Boolean,

    appendIcon: {
      type: String,
      default: '',
    },
    appendIconSection: {
      type: String,
      default: '',
    },
    appendIconSize: {
      type: String,
      default: '',
    },

    to: {
      type: [String, Object],
      default: "",
    },

    href: {
      type: [String, Object],
      default: "",
    },

    disabled: Boolean,
    loading: Boolean,

    type: {
      type: String,
      default: "button",
      validator: (v) => ["button", "submit", "reset"].includes(v),
    },
  },

  computed: {
    classes() {
      return {
        btn: true,
        [`btn--${this.variant}`]: this.variant,
        [`btn--${this.size}`]: this.size,
        "btn-is-variant": this.variant !== "default",
        "btn--loading": this.loading,
        "btn--disabled": this.disabled,
        'btn--icon': (this.icon || this.$slots.prepend) && !this.label && !this.$slots.default,
      };
    },
    styles() {
      return {
        padding: this.padding,
      };
    },
  },
};
</script>

<style lang="scss" scoped>
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0;

  font: {
    size: rem(12);
  }

  line-height: 1;
  color: $btn-color;
  text-decoration: none;
  cursor: pointer;
  background: none;
  border: none;
  border-radius: $border-radius-btn;

  transition: color $transition-timing-function $transition-duration,
  border-color $transition-timing-function $transition-duration,
  background $transition-timing-function $transition-duration,
  background-color $transition-duration $transition-timing-function,
  opacity $transition-timing-function $transition-duration,
  transform $transition-timing-function $transition-duration;

  &:not(&[disabled]):focus-visible {
    outline: $border-width solid $btn-color;
  }

  &-icon {
    display: flex;
    width: rem(20);
    height: rem(20);
    flex-shrink: 0;
    fill: currentColor;
  }

  &[disabled] {
    .u-icon.u-icon--clickable {
      cursor: not-allowed;
    }
  }

  &--primary {
    position: relative;
    z-index: 0;
    overflow: hidden;
    min-height: rem(40);
    max-width: rem(302);
    width: 100%;
    padding: rem(4) rem(16);
    color: $btn-color;
    background-color: rgba($btn-color, 0.2);
    border: $border-width solid transparent;
    
    &:not(&[disabled]):focus-visible, &:not(&[disabled]):hover {
      border-color: $btn-color;
      outline: none;
      background-color: rgba($btn-color, 0.15);
    }

    &[disabled] {
      cursor: not-allowed;
      background: linear-gradient(90deg, #888 0%, #B3B3B3 42.71%, #555 100%);
      border-color: $btn-disabled-border-color;
      color: $btn-disabled-color;
    }
  }

  &--secondary {
    position: relative;
    min-height: rem(48);
    max-width: rem(400);
    padding: rem(4) rem(16);
    text-transform: uppercase;
    color: $btn-color;
    background: rgba($btn-color, 0.2);
    background-clip: padding-box;
    border: $border-width-2x solid transparent;

    @include on-breakpoint('md') {
      min-height: rem(60);
      padding: rem(4) rem(20);
    }

    &:not(&[disabled])::before {
      content: "";
      position: absolute;
      top: -2px;
      right: -2px;
      bottom: -2px;
      left: -2px;
      z-index: -1;
      border-radius: $border-radius-btn;
      background: linear-gradient(90deg, #C9B075 0%, #D3C8B1 32.05%, #D3C8B1 57.29%, #B09D62 100%);
    }

    &:not(&[disabled])::after {
      content: "";
      position: absolute;
      top: -2px;
      bottom: -2px;
      left: -2px;
      width: 70%;
      z-index: -1;
      border-radius: $border-radius-btn;
      background: linear-gradient(90deg, transparent 0%, #FFF 50%, transparent 100%);
      transition: transform $transition-timing-function $transition-duration, opacity $transition-timing-function $transition-duration;
    }

    &:not(&[disabled]):hover::after {
      transform: translateX(45%);
    }

    &:not(&[disabled]):focus-visible {
      background-color: $btn-color;
      outline: none;
    }

    &[disabled] {
      cursor: not-allowed;
      background: linear-gradient(90deg, #888 0%, #797979 37.50%, #555 100%);
      border-color: $btn-disabled-border-color;
      color: $btn-disabled-color-two;
    }
  }

  .mr-2 {
    margin-right: rem(2);
  }

  .mr-5 {
    margin-right: rem(5);
  }

  .flex-shrink-0 {
    flex-shrink: 0;
  }

  .ml-auto {
    margin-left: auto;
  }
}
</style>
