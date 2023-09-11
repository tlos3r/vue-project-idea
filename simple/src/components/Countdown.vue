<template>
    <h1 class="mt-20 text-4xl font-bold text-center">Countdown</h1>
    <div v-if="loaded" class="text-center">
        <section class="flex flex-row items-center justify-center my-10 text-7xl">
            <div class="relative">
                {{ displayDay }}
            </div>
            <p class="px-5 leading-normal">:</p>
            <div class="relative">
                {{ displayHour }}
            </div>
            <p class="px-5 leading-normal">:</p>
            <div class="relative">
                {{ displayMinute }}
            </div>
            <p class="px-5 leading-normal">:</p>
            <div class="relative">
                {{ displaySecond }}
            </div>
        </section>
    </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
const displayDay = ref(0);
const displayHour = ref(0);
const displayMinute = ref(0);
const displaySecond = ref(0);
const loaded = ref(false);
const formatNum = (num: number): any => (num < 10 ? `0${num}` : num);
const _seconds = 1000;
const _minutes = _seconds * 60;
const _hours = _minutes * 60;
const _days = _hours * 60;
const timer = setInterval(() => {
    const now = new Date();
    const end = new Date(2024, 4, 10, 10, 10, 10);
    const distence = end.getTime() - now.getTime();
    if (distence < 0) {
        clearInterval(timer);
        loaded.value = true;
        return;
    }

    const days = Math.floor(distence / _days);
    const hours = Math.floor((distence % _days) / _hours);
    const minutes = Math.floor((distence % _hours) / _minutes);
    const seconds = Math.floor((distence % _minutes) / _seconds);
    displayDay.value = formatNum(days);
    displayHour.value = formatNum(hours);
    displayMinute.value = formatNum(minutes);
    displaySecond.value = formatNum(seconds);
    loaded.value = true;
}, 1000);
onMounted(() => {
    timer;
});
</script>

<style scoped></style>
