<!-- eslint-disable vue/no-mutating-props -->
<template>
  <v-row
    justify="start"
    align="center"
    style="gap: 5px"
  >
    <text-input
      :form-data="rule"
      :error="showError && !rule.from"
      key-attr="from"
      :disabled="isDisabled"
      label="original url"
    />

    <v-icon x-small>mdi-arrow-right</v-icon>

    <text-input
      :form-data="rule"
      :error="showError && !rule.to"
      key-attr="to"
      :disabled="isDisabled"
      label="redirect to"
    />

    <div
      class="d-flex align-center justify-center"
      style="width: 70px"
    >
      <template v-if="isDisabled">
        <v-switch
          x-small
          color="#149E8E"
          v-model="rule.active"
          @change="$emit('update')"
        />

        <v-menu offset-y>
          <template v-slot:activator="{ on, attrs }">
            <v-icon
              v-bind="attrs"
              v-on="on"
            >
              mdi-dots-vertical
            </v-icon>
          </template>

          <v-list>
            <v-list-item>
              <v-list-item-title
                class="menu-item"
                @click="isDisabled = false"
              >
                Edit
              </v-list-item-title>
            </v-list-item>

            <v-list-item>
              <v-list-item-title
                class="menu-item"
                @click="$emit('remove', rule)"
              >
                Remove
              </v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
      </template>

      <template v-else>
        <slot />

        <v-btn
          @click="save"
          color="#149E8E"
          :disabled="isInvalid"
          plain
          x-small
          min-height="66px"
        >
          <v-icon title="Save">mdi-content-save-outline</v-icon>
        </v-btn>
      </template>
    </div>
  </v-row>
</template>

<script>
import TextInput from './shared/TextInput.vue';

export default {
  name: 'RuleItem',

  components: {
    TextInput,
  },

  props: {
    disabled: {
      type: Boolean,
      default: true,
    },

    rule: {
      type: Object,
      default: () => ({
        id: null,
        from: null,
        to: null,
        active: false,
      }),
    },

    showError: {
      type: Boolean,
      default: true,
    },
  },

  data() {
    return {
      isDisabled: this.disabled,
    };
  },

  computed: {
    isInvalid() {
      return !this.rule.from || !this.rule.to;
    },
  },

  methods: {
    save() {
      if (this.isInvalid) return;
      this.$emit('update');
      this.isDisabled = true;
    },
  },
};
</script>
