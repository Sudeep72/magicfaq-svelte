<script lang="ts">
    import type { MagicFAQProps } from "./types";
  
    const props = $props();
    const { uid, position = "bottom-right" } = props as MagicFAQProps;
  
    let state = $state({
      isWidgetOpen: false,
      isMounted: false,
      isMobile: false
    });
  
    let iframeElement: HTMLIFrameElement;
  
  
    $effect(() => {
      const checkMobile = () => {
        state.isMounted = true;
        state.isMobile = window.innerWidth <= 768;
      };
  
      checkMobile();
      window.addEventListener("resize", checkMobile);
  
      const handleMessage = (event: MessageEvent<{ action: string }>) => {
        if (event.data.action === "close") {
          state.isWidgetOpen = false;
        }
      };
  
      window.addEventListener("message", handleMessage);
  
      return () => {
        window.removeEventListener("resize", checkMobile);
        window.removeEventListener("message", handleMessage);
      };
    });
  
    $effect(() => {
      if (iframeElement && state.isWidgetOpen !== undefined) {
        iframeElement.contentWindow?.postMessage(
          { action: state.isWidgetOpen ? "open" : "close" },
          "*"
        );
      }
    });
  
    $effect(() => {
      if (state.isWidgetOpen) {
        const handleKeyDown = (event: KeyboardEvent) => {
          if (event.key === "Escape") {
            window.parent.postMessage({ action: "close" }, "*");
            state.isWidgetOpen = false;
          }
        };
  
        document.addEventListener("keydown", handleKeyDown);
  
        return () => {
          document.removeEventListener("keydown", handleKeyDown);
        };
      }
    });
  
    function toggleWidget(): void {
      state.isWidgetOpen = !state.isWidgetOpen;
    }
  
    function handleKeyPress(e: KeyboardEvent): void {
      if (e.key === "Enter" || e.key === " ") {
        toggleWidget();
      }
    }
  
    function handleMouseEnter(e: MouseEvent): void {
      (e.currentTarget as HTMLDivElement).style.transform = "scale(1.05)";
    }
  
    function handleMouseLeave(e: MouseEvent): void {
      (e.currentTarget as HTMLDivElement).style.transform = state.isWidgetOpen ? "scale(0.9)" : "scale(1)";
    }
  </script>
  
  <iframe
    src={`https://magicfaq.vercel.app/?mid=${uid}`}
    title="Magic FAQ Widget"
    width="100%"
    height="100%"
    allow="encrypted-media"
    allowfullscreen
    style="
      border: none;
      position: fixed;
      bottom: 0px;
      right: 0px;
      z-index: 1000;
      display: {state.isWidgetOpen ? 'block' : 'none'};
    "
    bind:this={iframeElement}
  ></iframe>
  
  <button
    type="button"
    onclick={toggleWidget}
    onkeypress={handleKeyPress}
    onmouseenter={handleMouseEnter}
    onmouseleave={handleMouseLeave}
    aria-label={state.isWidgetOpen ? "Close FAQ" : "Open FAQ"}
    aria-expanded={state.isWidgetOpen}
    style="
      position: fixed;
      {position.includes('top') ? 'top: 24px;' : 'bottom: 24px;'} 
      {position.includes('left') ? 'left: 24px;' : 'right: 24px;'} 
      width: 64px;
      height: 64px;
      color: white;
      font-size: 2rem;
      display: flex;
      align-items: center;
      justify-content: center;
      background-color: #007bff;
      border-radius: 50%;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
      cursor: pointer;
      transition: all 0.3s ease;
      z-index: {state.isMobile ? 99 : 9999};
      opacity: {state.isMounted ? 1 : 0};
      transform: {state.isWidgetOpen ? 'scale(0.8)' : 'scale(1)'};
      border: none;
      padding: 0;
    "
  >
    <div
      style="
        transition: all 0.3s ease;
        transform: {state.isWidgetOpen ? 'scale(0.9)' : 'scale(1)'};
      "
    >
      {#if state.isWidgetOpen}
        <svg fill="#fff" width="24px" height="24px" viewBox="-1 0 19 19" xmlns="http://www.w3.org/2000/svg">
          <path d="M8.5 15.313a1.026 1.026 0 0 1-.728-.302l-6.8-6.8a1.03 1.03 0 0 1 1.455-1.456L8.5 12.828l6.073-6.073a1.03 1.03 0 0 1 1.455 1.456l-6.8 6.8a1.026 1.026 0 0 1-.728.302z"></path>
        </svg>
      {:else}
        <svg fill="#fff" width="24px" height="24px" viewBox="0 0 512 512" xmlns="http://www.w3.org/2000/svg">
          <title>Chat</title>
          <path d="M96 368Q83 368 74 359 64 349 64 336L64 128Q64 114 74 105 83 96 96 96L416 96Q430 96 439 105 448 114 448 128L448 336Q448 349 439 359 430 368 416 368L256 368 160 464 160 368 96 368Z"></path>
        </svg>
      {/if}
    </div>
  </button>
  