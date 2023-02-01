<script lang="ts">
export default {
	name: 'SkeletonCard',
};
</script>

<template>
	<div class="skeleton-bg rounded-lg h-56 px-4 py-6 flex items-center justify-between flex-col">
		<span class="h-12 w-full rounded loading"></span>
		<span class="h-full w-full rounded loading mt-8"></span>
	</div>
</template>

<style lang="scss">
	@keyframes GradientBackground {
		to {
			background-position: 400% 0;
		}
	}
	.loading {
		animation: GradientBackground 5000ms infinite linear reverse;
        background: linear-gradient(110deg, #e2e8f0, #cbd5e1, #e2e8f0) 0 0 / 400% 100%;
	}

	.skeleton-bg {
		background: #f1f5f9;
	}
</style>
