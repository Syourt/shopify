<template>
  <div>
    <p class="name">{{ address.firstName }} {{ address.lastName }}</p>
    <p>{{ street }}</p>

    <p>
      {{ address.city }}
      {{ address.state }}
      {{ address.postalCode }}
    </p>

    <p>{{ country }}</p>
    <p v-if="address.phone">T: {{ address.phone }}</p>
  </div>
</template>

<script>
import { toRef, computed } from '@vue/composition-api';
import { getSettings } from '@vue-storefront/shopify-api';

export default {
  props: {
    address: {
      type: Object,
      required: true
    }
  },

  setup(props) {
    const address = toRef(props, 'address');

    const street = computed(() => {
      const parts = [
        address.streetName,
        address.streetNumber && ` ${ address.streetNumber }`,
        address.apartment && `, Apartment ${ address.apartment }`
      ];

      return parts.filter(Boolean).join('');
    });

    const country = computed(() => {
      const country = address.country;
      return getSettings().countries.find(c => c.name === country)?.label || country;
    });

    return {
      street,
      country
    };
  }
};
</script>

<style lang="scss" scoped>
p {
  margin: 0;
}

.name {
  font-weight: var(--font-weight--semibold);
}
</style>
