<script setup lang="ts">
import Toolbar from 'primevue/toolbar';

let resourceTypes = defineModel("resourceTypes");
let selectedResourceType = ref("Patient");

</script>

<template>

    <Toolbar>
        <template #start>
            <Button icon="pi pi-print" class="mr-2" severity="secondary" text />
            <Button icon="pi pi-upload" severity="secondary" text />
        </template>

        <template #center>
            <IconField>
                <InputIcon>
                    <i class="pi pi-search" />
                </InputIcon>
                <InputText placeholder="Search" />
            </IconField>
        </template>
        
        <template #end>
            <Select v-model="selectedResourceType" :options="resourceTypes" placeholder="種類を選択"
                class="w-full md:w-56" />
        </template>
    </Toolbar>
</template>