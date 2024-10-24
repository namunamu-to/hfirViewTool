<script setup lang="ts">
import FloatLabel from 'primevue/floatlabel';
// const keyStr = defineModel('keyStr')
// const valStr = defineModel('valStr')
const rowJson = defineModel('rowJson');
</script>

<template>
    <div v-for="(val, key) in rowJson" style="display: inline-block;">
        <FloatLabel variant="on" class="row">
            <label for={{key}}>{{ key }}</label>
            <InputText v-model=rowJson[key] autocomplete="off" readonly="true" />
        </FloatLabel>
    </div>
</template>
