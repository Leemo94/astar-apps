<template>
  <div class="card">
    <div class="card--title">{{ caption }}</div>
    <div v-if="eras" class="card--days">{{ eras }} days</div>
    <div class="card--balance">
      <div class="card--amount">
        {{ $n(truncate(ethers.utils.formatEther(amount.toString()) ?? '0', 2)) }}
      </div>
      <div class="card--symbol">{{ nativeTokenSymbol }}</div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { ethers } from 'ethers';
import { useNetworkInfo } from 'src/hooks';
import { truncate } from '@astar-network/astar-sdk-core';

export default defineComponent({
  props: {
    caption: {
      type: String,
      required: true,
    },
    amount: {
      type: BigInt as unknown as PropType<BigInt>,
      required: true,
    },
    eras: {
      type: Number,
      required: false,
      default: 0,
    },
  },
  setup() {
    const { nativeTokenSymbol } = useNetworkInfo();

    return {
      nativeTokenSymbol,
      ethers,
      truncate,
    };
  },
});
</script>

<style lang="scss" scoped>
.card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  z-index: 1;
  position: relative;
}

.card--title {
  font-size: 14px;
  font-weight: 700;
  flex: 1;
  position: relative;
}

.card--balance {
  display: flex;
  align-items: flex-end;
  gap: 8px;
}

.card--amount {
  font-size: 16px;
  font-weight: 700;
}

.card--symbol {
  font-size: 14px;
  font-weight: 600;
}
</style>
