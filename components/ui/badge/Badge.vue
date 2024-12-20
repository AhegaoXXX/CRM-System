<script setup lang="ts">
import { cn } from '@/utils';
import type { VariantProps } from 'class-variance-authority';
import { badgeVariants } from '.';

// eslint-disable-next-line @typescript-eslint/no-empty-object-type
interface BadgeVariantProps extends VariantProps<typeof badgeVariants> {}

interface Props {
  variant?: BadgeVariantProps['variant'];
}
defineProps<Props>();
</script>

<template>
  <div :class="cn(badgeVariants({ variant }), $attrs.class ?? '')">
    <slot />
  </div>
</template>
