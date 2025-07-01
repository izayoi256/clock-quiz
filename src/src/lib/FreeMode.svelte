<script lang="ts">
import { createEventDispatcher } from 'svelte';
import Clock from './Clock.svelte';

const dispatch = createEventDispatcher();

let hour = $state(0);
let minute = $state(0);

const setDate = (date: Date) => {
  hour = date.getHours() % 12;
  if (hour === 0) {
    hour = 12;
  }
  minute = date.getMinutes();
};

const addHour = () => {
  const date = new Date(2000, 0, 0, hour, minute);
  date.setHours(date.getHours() + 1);
  setDate(date);
};

const reduceHour = () => {
  const date = new Date(2000, 0, 0, hour, minute);
  date.setHours(date.getHours() - 1);
  setDate(date);
};

const addMinute = () => {
  const date = new Date(2000, 0, 0, hour, minute);
  date.setMinutes(date.getMinutes() + 1);
  setDate(date);
};

const reduceMinute = () => {
  const date = new Date(2000, 0, 0, hour, minute);
  date.setMinutes(date.getMinutes() - 1);
  setDate(date);
};

const complete = () => {
  dispatch('complete');
};

setDate(new Date());
</script>

<Clock hour={hour} minute={minute} />

<h2>{hour}じ {minute}ふん</h2>

<h1 class="mt-4">とけいをうごかしてみよう</h1>

<div class="mt-2 grid grid-cols-2 gap-4">
  <button class="!bg-gray-300" onclick={addHour}>1じかん ふやす</button>
  <button class="!bg-gray-300" onclick={addMinute}>1ふん ふやす</button>
  <button class="!bg-gray-300" onclick={reduceHour}>1じかん へらす</button>
  <button class="!bg-gray-300" onclick={reduceMinute}>1ふん へらす</button>
</div>

<div class="mt-4 grid grid-cols-1 gap-4">
  <button onclick={complete}>メニューにもどる</button>
</div>
