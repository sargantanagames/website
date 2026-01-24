<script lang="ts">
	import { onMount, onDestroy } from 'svelte';

	import idleGif from '$lib/assets/vpetlings/vpet-idle.gif';
	import walkGif from '$lib/assets/vpetlings/vpet-walk.gif';

	const { featureImage } = $props<{
		featureImage: HTMLImageElement | null;
	}>();

	const BASE_SPEED = 0.0003;
	const IDLE_DURATION = 1000;
	const IDLE_LOOPS = 3;
	const MOUSE_IDLE_RADIUS = 120;

	const SPAWN_X = 0.09;
	const SPAWN_Y = 0.48;

	/* ───────────────── DOM refs ───────────────── */

	let container: HTMLDivElement | null = null;
	let raf: number;

	let x = $state(-10000);
	let y = $state(-10000);
	let targetX = $state(0);
	let targetY = $state(0);

	let state = $state<'idle' | 'walk'>('idle');
	let idleElapsed = $state(0);

	let mouseX = $state(0);
	let mouseY = $state(0);
	let lastMouseX = 0;
	let lastMouseY = 0;
	let mouseTravel = $state(0);

	let lastTime = 0;
	let facing = $state<'left' | 'right'>('left');

	let isActive = $state(false);
	let firstActivation = true;

	let imageWidth = $state(0);

	const petSize = $derived(imageWidth > 0 ? imageWidth * 0.17 : 144);


	function handleMouseMove(e: MouseEvent): void {
		if (lastMouseX !== 0 || lastMouseY !== 0) {
			mouseTravel += Math.hypot(
				e.clientX - lastMouseX,
				e.clientY - lastMouseY
			);
		}

		lastMouseX = e.clientX;
		lastMouseY = e.clientY;
		mouseX = e.clientX;
		mouseY = e.clientY;

		if (isActive && mouseTravel >= petSize * 3) {
			mouseTravel = 0;
			idleElapsed = 0;
			pickTarget();
		}
	}

	function spawnFromImage(nx: number, ny: number): void {
		if (!featureImage || !container) return;

		const imageRect = featureImage.getBoundingClientRect();
		const containerRect = container.getBoundingClientRect();

		imageWidth = imageRect.width;

		x = imageRect.left + imageRect.width * nx - containerRect.left;
		y = imageRect.top + imageRect.height * ny - containerRect.top;

		targetX = x;
		targetY = y;

		mouseTravel = 0;
		lastMouseX = 0;
		lastMouseY = 0;
	}

	function isMouseNear(): boolean {
		if (!container) return false;

		const rect = container.getBoundingClientRect();
		const petScreenX = rect.left + x;
		const petScreenY = rect.top + y;

		return Math.hypot(
			mouseX - petScreenX,
			mouseY - petScreenY
		) < petSize * 3;
	}

	function pickTarget(): void {
		if (!container) return;

		const rect = container.getBoundingClientRect();
		if (rect.width === 0 || rect.height === 0) {
			state = 'idle';
			return;
		}

		const minX = petSize / 2;
		const minY = petSize / 2;
		const maxX = rect.width - petSize / 2;
		const maxY = rect.height - petSize / 2;

		const originX = mouseX || rect.left + rect.width / 2;
		const originY = mouseY || rect.top + rect.height / 2;

		const angle = Math.random() * Math.PI * 2;
		const distance = Math.random() * MOUSE_IDLE_RADIUS;

		let tx = originX + Math.cos(angle) * distance - rect.left;
		let ty = originY + Math.sin(angle) * distance - rect.top;

		targetX = Math.min(maxX, Math.max(minX, tx));
		targetY = Math.min(maxY, Math.max(minY, ty));

		state = 'walk';
	}

	/* ───────────────── animation loop ───────────────── */

	function loop(time: number): void {
		raf = requestAnimationFrame(loop);
		if (!isActive || !container) return;

		const dt = time - lastTime;
		lastTime = time;

		if (state === 'idle') {
			if (isMouseNear()) {
				idleElapsed = 0;
			} else {
				idleElapsed += dt;

				const effectiveIdle =
					IDLE_DURATION *
					(firstActivation ? IDLE_LOOPS / 4 : IDLE_LOOPS);

				if (idleElapsed >= effectiveIdle) {
					idleElapsed = 0;
					firstActivation = false;
					pickTarget();
				}
			}
		}

		if (state === 'walk') {
			const dx = targetX - x;
			const dy = targetY - y;
			const dist = Math.hypot(dx, dy);

			if (Math.abs(dx) > 1) {
				facing = dx < 0 ? 'left' : 'right';
			}

			if (dist < 10) {
				x = targetX;
				y = targetY;
				state = 'idle';
				idleElapsed = 0;
			} else {
				const rect = container.getBoundingClientRect();
				const speed =
					BASE_SPEED * Math.min(rect.width, rect.height);

				x += (dx / dist) * speed * dt;
				y += (dy / dist) * speed * dt;
			}
		}
	}

	/* ───────────────── lifecycle ───────────────── */

	function updateContainerHeight(): void {
		if (!container || !isActive) return;
		container.style.height = `${document.body.scrollHeight}px`;
	}

	function handleResize(): void {
		updateContainerHeight();
	}

	function reparentToWorld(): void {
		if (!container) return;
		const world = container.parentElement?.parentElement;
		if (world) world.appendChild(container);
	}

	onMount(() => {
		const waitForAnchor = () => {
			if (container && featureImage) {
				spawnFromImage(SPAWN_X, SPAWN_Y);

				const revealEl =
					featureImage.closest('.reveal') as HTMLElement | null;

				const observer = new IntersectionObserver(
					([entry]) => {
						if (entry.isIntersecting && !isActive && revealEl) {
							revealEl.addEventListener(
								'transitionend',
								(e) => {
									if (e.target !== revealEl) return;
									isActive = true;
									lastTime = performance.now();
									reparentToWorld();
									spawnFromImage(SPAWN_X, SPAWN_Y);
									updateContainerHeight();
								},
								{ once: true }
							);
						}
					},
					{ threshold: 1 }
				);

				observer.observe(featureImage);
				window.addEventListener('pointermove', handleMouseMove);
				window.addEventListener('resize', handleResize);

				raf = requestAnimationFrame(loop);
				return;
			}

			requestAnimationFrame(waitForAnchor);
		};

		waitForAnchor();
	});

	onDestroy(() => {
		if (container && container.parentNode) {
			container.parentNode.removeChild(container);
		}

		if (typeof window !== 'undefined') {
			window.cancelAnimationFrame(raf);
			window.removeEventListener('pointermove', handleMouseMove);
			window.removeEventListener('resize', handleResize);
		}

	});
</script>

<div
	bind:this={container}
	class="pointer-events-none absolute top-0 left-0 z-50 w-full"
>
	<img
		src={state === 'idle' ? idleGif : walkGif}
		alt="Virtual pet"
		class="pointer-events-none absolute select-none"
		style="
			width: {petSize}px;
			height: {petSize}px;
			transform:
				translate({x}px, {y}px)
				translate(-50%, -50%)
				scaleX({facing === 'left' ? -1 : 1});
		"
	/>
</div>
