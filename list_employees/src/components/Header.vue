<template>
    <header>
        <h1>{{title}}</h1>
        <div>
            <Button @btn-click="$emit('toggle-filter')" class="btn"
                    :text="showFilter ? 'Hide filter' : 'Show filter'"
                    :color="showFilter ? '#Af473C' : '#00c281'"/>
        </div>
    </header>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Button from './Button.vue'

export default defineComponent({
    name: 'Header',
    components: {
        Button,
    },
    props: {
        title: String,
        showFilter: Boolean,
    },
})
</script>

<style scoped>
header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}
</style>