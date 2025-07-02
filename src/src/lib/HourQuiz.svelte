<script lang="ts">
import { createEventDispatcher } from 'svelte';
import confetti from 'canvas-confetti';
import Clock from './Clock.svelte';

const dispatch = createEventDispatcher();

let correctHour = $state(0);
let correctMinute = $state(0);
let choices = $state([]);
let chose = $state(false);

const nextLevel = () => {
  correctHour = Math.floor(Math.random() * 12) + 1;
  correctMinute = Math.floor(Math.random() * 60);

  const newAnswersMap = new Map<string, [number, boolean]>();
  while (true) {
    const hour = correctHour + Math.floor(Math.random() * 13) - 6;
    if (hour === correctHour || hour < 1 || 12 < hour ) {
      continue;
    }
    newAnswersMap.set(hour.toString(), [hour, false]);
    if (newAnswersMap.size > 3) {
      break;
    }
  }
  const newChoices = Array.from(newAnswersMap.values());
  newChoices[Math.floor(Math.random() * newAnswersMap.size)] = [correctHour, true];
  choices = newChoices;
  chose = false;
}

nextLevel();

const complete = () => {
  dispatch('complete');
}

const onChooseAnswer = (correct: boolean) => {
  if (chose) {
    return;
  }
  chose = true;
  if (correct) {
    confetti({
      particleCount: 100,
      spread: 70,
      origin: { y: 0.6 },
    });
  }
}
</script>

<Clock hour={correctHour} minute={correctMinute} />

<h1>なんじかな？</h1>
<div class="mt-2 grid grid-cols-2 gap-4">
  {#each choices as choice}
    <button
        class:!bg-gray-300={!chose}
        class:!bg-gray-500={chose && !choice[1]}
        class:!bg-yellow-300={chose && choice[1]}
        onclick={() => onChooseAnswer(choice[1])}
    >
      {choice[0]} じ
    </button>
  {/each}
</div>

<div class="mt-4 grid grid-cols-1 gap-4">
  <button onclick={nextLevel}>つぎのもんだい</button>
  <button onclick={complete}>メニューにもどる</button>
</div>
