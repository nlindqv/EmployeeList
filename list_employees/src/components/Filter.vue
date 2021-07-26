<template>
    <div class="filter">
    <div class="slider">
        <label>Age</label>
        <Slider @update="$emit('set-age-range', filteredAgeRange)"
                v-model="filteredAgeRange" :min="ageRange[0]" :max="ageRange[1]"/>
    </div>
    <div class="slider">
        <label>Salary</label>
        <Slider @update="$emit('set-salary-range', filteredSalaryRange)"
                v-model="filteredSalaryRange" :min="salaryRange[0]" :max="salaryRange[1]"
                :format="format"
                :step="100"/>
    </div>
    </div>
</template>

<script lang="ts">
import Slider from '@vueform/slider';
import { defineComponent, PropType } from 'vue';

export default defineComponent({
    name: 'Filter',
    props: {
        ageRange: Array as PropType<Array<number>>,
        salaryRange: Array as PropType<Array<number>>,
    },
    components: {
        Slider,
    },
    data() {
        return {
            filteredSalaryRange: this.salaryRange ? [...this.salaryRange]: [0, 1000000],
            filteredAgeRange: this.ageRange ? [...this.ageRange] : [0, 100],
            /** Format for salary slider */
            format: {       
                prefix: '$',
                thousand: ',',
                decimals: '0'
            }
        }
    },
})
</script>

<style src="@vueform/slider/themes/default.css"/>
