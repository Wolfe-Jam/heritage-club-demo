<!-- RolexBackground.svelte -->
<script lang="ts">
  import { onMount } from 'svelte';
  
  let animationStarted = $state(false);
  let watchContainer: HTMLDivElement;

  onMount(() => {
    // Start zoom animation after mount
    setTimeout(() => {
      animationStarted = true;
    }, 300);
  });
</script>

<!-- Watch Background Container -->
<div class="watch-app-background">
  <div 
    bind:this={watchContainer}
    class="rolex-photo-container"
    class:zoom-complete={animationStarted}
  >
    <!-- Your Rolex Photo (WebP optimized) -->
    <img 
      src="/rolex-sky-dweller.webp" 
      alt="Rolex Sky-Dweller"
      class="rolex-photo"
    />
    
    <!-- Light Sweep Overlay -->
    <div class="light-sweep"></div>
    
    <!-- Ambient Glow Effect -->
    <div class="ambient-glow"></div>
  </div>
</div>

<style>
  .watch-app-background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: radial-gradient(circle at center, #1a1a1a 0%, #000000 100%);
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: -1;
  }

  .rolex-photo-container {
    position: relative;
    width: clamp(300px, 50vw, 600px);
    height: clamp(300px, 50vw, 600px);
    transform: scale(15) translateZ(0);
    opacity: 0;
    transition: all 3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
    will-change: transform, opacity;
  }

  .rolex-photo-container.zoom-complete {
    transform: scale(1) translateZ(0);
    opacity: 0.3;
  }

  .rolex-photo {
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 50%;
    filter: contrast(1.1) brightness(0.9) saturate(1.2);
    box-shadow: 
      0 0 50px rgba(255, 255, 255, 0.1),
      0 20px 40px rgba(0, 0, 0, 0.6);
  }

  .light-sweep {
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(
      45deg,
      transparent 0%,
      transparent 40%,
      rgba(255, 255, 255, 0.15) 50%,
      transparent 60%,
      transparent 100%
    );
    border-radius: 50%;
    animation: lightSweep 8s ease-in-out infinite;
    pointer-events: none;
  }

  .ambient-glow {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 120%;
    height: 120%;
    transform: translate(-50%, -50%);
    background: radial-gradient(
      circle,
      rgba(255, 215, 0, 0.05) 0%,
      rgba(255, 255, 255, 0.02) 50%,
      transparent 70%
    );
    border-radius: 50%;
    animation: ambientPulse 6s ease-in-out infinite;
    pointer-events: none;
  }

  @keyframes lightSweep {
    0% { left: -100%; opacity: 0; }
    5% { opacity: 1; }
    15% { left: 100%; opacity: 1; }
    20% { opacity: 0; }
    100% { left: 100%; opacity: 0; }
  }

  @keyframes ambientPulse {
    0%, 100% { 
      opacity: 0.6; 
      transform: translate(-50%, -50%) scale(1); 
    }
    50% { 
      opacity: 0.9; 
      transform: translate(-50%, -50%) scale(1.1); 
    }
  }

  @media (max-width: 768px) {
    .rolex-photo-container {
      width: 70vw;
      height: 70vw;
      max-width: 400px;
      max-height: 400px;
    }
    
    .rolex-photo-container.zoom-complete {
      opacity: 0.25;
    }
  }

  @media (prefers-reduced-motion: reduce) {
    .rolex-photo-container {
      transition: opacity 1s ease;
      transform: scale(1) translateZ(0);
    }
    
    .light-sweep,
    .ambient-glow {
      animation: none;
    }
  }
</style>