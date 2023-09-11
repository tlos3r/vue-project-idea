<template>
    <main class="mt-24">
        <div class="container mt-20 calculator">
            <div class="display">{{ current || "0" }}</div>
            <div @click="clear" class="btn">C</div>
            <div @click="sign" class="btn">+/-</div>
            <div @click="percent" class="btn">%</div>
            <div @click="append('/')" class="btn operator">÷</div>
            <div @click="append('7')" class="btn">7</div>
            <div @click="append('8')" class="btn">8</div>
            <div @click="append('9')" class="btn">9</div>
            <div @click="append('*')" class="btn operator">x</div>
            <div @click="append('4')" class="btn">4</div>
            <div @click="append('5')" class="btn">5</div>
            <div @click="append('6')" class="btn">6</div>
            <div @click="append('-')" class="btn operator">-</div>
            <div @click="append('1')" class="btn">1</div>
            <div @click="append('2')" class="btn">2</div>
            <div @click="append('3')" class="btn">3</div>
            <div @click="append('+')" class="btn operator">+</div>
            <div @click="append('0')" class="btn zero">0</div>
            <div @click="dot" class="btn">.</div>
            <div @click="equal" class="btn">=</div>
        </div>
    </main>
</template>

<script setup lang="ts">
import { ref } from "vue";
const current = ref("");

const clear = () => (current.value = "");
const sign = () => (current.value = current.value.charAt(0) === "-" ? current.value.slice(1) : `-${current.value}`);
const percent = () => (current.value = `${parseFloat(current.value) / 100}`);
const append = (num: string) => {
    current.value = `${current.value}${num}`;
};

const dot = () => {
    if (current.value.indexOf(".") === -1) {
        append(".");
    }
};

const equal = () => {
    try {
        const evalFunction = new Function(`return ${current.value};`);
        current.value = evalFunction();
    } catch (error) {
        console.error(error);
    }
};
</script>

<style scoped>
.calculator {
    @apply grid grid-cols-[repeat(4,1fr)] auto-rows-[minmax(50px,auto)] text-4xl text-center w-96 my-0 mx-auto;
}
.display {
    @apply col-[1/5] bg-[#333] text-white py-3;
}
.zero {
    @apply col-[1/3];
}
.btn {
    @apply bg-[#f2f2f2] border border-solid border-[#999] py-3 cursor-pointer;
}
.operator {
    @apply bg-orange-400 text-white;
}
</style>
