<script lang="ts">
	import { onMount, onDestroy } from 'svelte';

	import idleGif from '$lib/assets/vpetlings/vpet-idle.gif';
	import walkGif from '$lib/assets/vpetlings/vpet-walk.gif';
	import petGif from '$lib/assets/vpetlings/vpet-pet.gif';

	const { featureImage } = $props<{
		featureImage: HTMLImageElement | null;
	}>();

	const BASE_SPEED = 0.0003;
	const IDLE_DURATION = 1000;
	const IDLE_LOOPS = 3;

	const SPAWN_X = 0.09;
	const SPAWN_Y = 0.48;

	let container: HTMLDivElement | null = null;
	let raf: number;

	let x = $state(-10000);
	let y = $state(-10000);
	let targetX = $state(0);
	let targetY = $state(0);

	let state = $state<'idle' | 'walk' | 'petting'>('idle');
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

	const PET_PET_DURATION = 1600;
	const DEFAULT_ARRIVAL_THRESHOLD = 10;
	const SMOOTHING = 0.2;

	let currentDirection = { x: 0, y: 0 };
	let currentArrivalThreshold = DEFAULT_ARRIVAL_THRESHOLD;


	function handleClick(e: MouseEvent): void {
		state = 'petting';
		idleElapsed = 0;

		setTimeout(() => {
			state = 'idle';
		}, PET_PET_DURATION);
	}

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
		if (!container || state == 'petting') return;

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
		const distance = petSize;

		let tx = originX + Math.cos(angle) * distance - rect.left;
		let ty = originY + Math.sin(angle) * distance - rect.top;

		tx = Math.min(maxX, Math.max(minX, tx));
		ty = Math.min(maxY, Math.max(minY, ty));

		setTarget(tx, ty);
		state = 'walk';
	}

	function setTarget(tx: number, ty: number, arrivalOverride: number = -1) {
		targetX = tx;
		targetY = ty;
		currentArrivalThreshold =
			arrivalOverride >= 0 ? arrivalOverride : DEFAULT_ARRIVAL_THRESHOLD;
		if (Math.hypot(currentDirection.x, currentDirection.y) < 0.001) {
			const dx = targetX - x;
			const dy = targetY - y;
			const d = Math.hypot(dx, dy) || 1;
			currentDirection.x = dx / d;
			currentDirection.y = dy / d;
		}
	}

	function loop(time: number): void {
		raf = requestAnimationFrame(loop);
		if (!isActive || !container) return;

		const dt = time - lastTime;
		lastTime = time;

		if (state === 'idle') {
			if (!firstActivation && isMouseNear()) {
				idleElapsed = 0;
			} else {
				idleElapsed += dt;

				const effectiveIdle =
					IDLE_DURATION *
					(firstActivation ? 0 : IDLE_LOOPS);

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

			let desiredX = 0;
			let desiredY = 0;
			if (dist > 0.0001) {
				desiredX = dx / dist;
				desiredY = dy / dist;
			}

			currentDirection.x += (desiredX - currentDirection.x) * SMOOTHING;
			currentDirection.y += (desiredY - currentDirection.y) * SMOOTHING;

			const cdLen = Math.hypot(currentDirection.x, currentDirection.y) || 1;
			currentDirection.x /= cdLen;
			currentDirection.y /= cdLen;

			if (Math.abs(currentDirection.x) >= 0.01) {
				facing = currentDirection.x < 0 ? 'left' : 'right';
			}

			const rect = container.getBoundingClientRect();
			const speed = BASE_SPEED * Math.min(rect.width, rect.height);

			x += currentDirection.x * speed * dt;
			y += currentDirection.y * speed * dt;

			if (Math.hypot(targetX - x, targetY - y) < currentArrivalThreshold) {
				x = targetX;
				y = targetY;
				state = 'idle';
				idleElapsed = 0;
				currentDirection.x = 0;
				currentDirection.y = 0;
			} else {
				enforceBounds();
			}
		}
	}

	function enforceBounds(): void {
		if (!container) return;

		const rect = container.getBoundingClientRect();

		const minX = 0;
		const minY = 0;
		const maxX = rect.width;
		const maxY = rect.height;

		let teleported = false;

		if (x < minX) {
			x = minX;
			teleported = true;
		} else if (x > maxX) {
			x = maxX;
			teleported = true;
		}

		if (y < minY) {
			y = minY;
			teleported = true;
		} else if (y > maxY) {
			y = maxY;
			teleported = true;
		}

		if (teleported) {
			currentDirection.x = 0;
			currentDirection.y = 0;
		}
	}


	function updateContainerHeight(): void {
		if (!container || !isActive) return;
		console.log(`From height: ${container.getBoundingClientRect().height}px to ${document.body.scrollHeight}px`);
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
		src={state === 'petting' ? petGif : state === 'idle' ? idleGif : walkGif}
		alt="Virtual pet"
		class="pointer-events-auto absolute select-none cursor-pointer"
		on:click={handleClick}
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
