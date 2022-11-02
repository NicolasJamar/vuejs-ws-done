<script setup>

defineProps({
    details: Array
});

</script>

<template>

    <div class="list">
        <ul>
            <li v-for="detail in details">
                {{ detail }}
            </li>
        </ul>
    </div>

</template>