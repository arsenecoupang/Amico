<script>
	import { Sparkles } from 'lucide-svelte';

	export let size = 'md';
	export let showIcon = true;
	export let className = '';

	const sizeClasses = {
		xs: {
			text: 'text-sm',
			icon: 'w-3 h-3',
			container: 'gap-1'
		},
		sm: {
			text: 'text-lg',
			icon: 'w-4 h-4',
			container: 'gap-1'
		},
		md: {
			text: 'text-2xl',
			icon: 'w-5 h-5',
			container: 'gap-2'
		},
		lg: {
			text: 'text-3xl',
			icon: 'w-6 h-6',
			container: 'gap-2'
		}
	};

	function isValidSize(s) {
		return s && typeof s === 'string' && s in sizeClasses;
	}

	$: validSize = isValidSize(size) ? size : 'md';
	$: styles = sizeClasses[validSize];
</script>

<div class={`flex items-center ${styles.container} ${className}`}>
	{#if showIcon}
		<Sparkles class={`${styles.icon} text-[var(--soft-blue)]`} />
	{/if}
	<span
		class={`${styles.text} bg-gradient-to-r from-[var(--soft-blue)] to-[var(--soft-purple)] bg-clip-text font-bold text-transparent`}
	>
		Amico
	</span>
</div>
