<script lang="ts">
import { createEventDispatcher } from 'svelte';
import Clock from './Clock.svelte';
import confetti from 'canvas-confetti';

const dispatch = createEventDispatcher();

let correctHour = $state(0);
let correctMinute = $state(0);
let choices = $state([]);
let chose = $state(false);

const nextLevel = () => {
  correctHour = Math.floor(Math.random() * 12) + 1;
  correctMinute = Math.floor(Math.random() * 60);

  const newAnswersMap = new Map<string, [number, number, boolean]>();
  while (true) {
    const hour = correctHour + Math.floor(Math.random() * 3) - 1;
    const minute = correctMinute + Math.floor(Math.random() * 11) - 5;
    if ((hour === correctHour && minute === correctMinute) || hour < 1 || 12 < hour || minute < 0 || 59 < minute ) {
      continue;
    }
    newAnswersMap.set(`${hour.toString()}-${minute.toString()}`, [hour, minute, false]);
    if (newAnswersMap.size > 3) {
      break;
    }
  }
  const newChoices = Array.from(newAnswersMap.values());
  newChoices[Math.floor(Math.random() * newAnswersMap.size)] = [correctHour, correctMinute, true];
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

<h1>なんじなんふんかな？</h1>
<div class="mt-2 grid grid-cols-2 gap-4">
  {#each choices as choice}
    <button
        class:!bg-gray-300={!chose}
        class:!bg-gray-500={chose && !choice[2]}
        class:!bg-yellow-300={chose && choice[2]}
        onclick={() => onChooseAnswer(choice[2])}
    >
      {choice[0]} じ
      {choice[1]} ふん
    </button>
  {/each}
</div>

<div class="mt-4 grid grid-cols-1 gap-4">
  <button onclick={nextLevel}>つぎのもんだい</button>
  <button onclick={complete}>メニューにもどる</button>
</div>
