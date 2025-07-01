<script lang="ts">
  import Menu from './lib/Menu.svelte';
  import HourQuiz from './lib/HourQuiz.svelte';
  import MinuteQuiz from './lib/MinuteQuiz.svelte';
  import HourMinuteQuiz from './lib/HourMinuteQuiz.svelte';
  import FreeMode from './lib/FreeMode.svelte';
  let currentMode = $state('menu');
  const handleModeSelect = (e) => {
    currentMode = e.detail;
  };
  const handleQuizComplete = (e) => {
    currentMode = 'menu';
  };
</script>

<main>
  {#if currentMode === 'menu'}
    <Menu on:selectMode={handleModeSelect} />
  {:else if currentMode === 'hour'}
    <HourQuiz on:complete={handleQuizComplete} />
  {:else if currentMode === 'minute'}
    <MinuteQuiz on:complete={handleQuizComplete} />
  {:else if currentMode === 'both'}
    <HourMinuteQuiz on:complete={handleQuizComplete} />
  {:else if currentMode === 'free'}
    <FreeMode on:complete={handleQuizComplete} />
  {/if}
</main>
