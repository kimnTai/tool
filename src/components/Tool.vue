<script setup lang="ts">
import { ref } from "vue";
const before = ref(``);
const after = ref("");

function dev() {
    const _map = new Map();
    before.value
        .split("\n")
        .filter((v) => v.includes(":"))
        .filter(([v]) => !isNaN(+v))
        .forEach((v) => {
            const [time, ...args] = v.split(" ");
            if (_map.has(time)) {
                _map.set(time, [..._map.get(time), ...args]);
            } else {
                _map.set(time, args);
            }
        });

    const arr = [..._map.entries()].reduce((pre, [key, value]) => {
        pre[key] = value;
        return pre;
    }, {} as any);
    after.value = JSON.stringify(arr, null, 2);
}
</script>

<template>
    <div>
        <h3>原軸</h3>
        <textarea v-model="before" cols="30" rows="10"></textarea>
    </div>
    <div>
        <h3>編譯後</h3>
        <textarea v-model="after" cols="30" rows="10"></textarea>
    </div>
    <button @click="dev">dev</button>
</template>
