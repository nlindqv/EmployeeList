// Component to present an employee as a row in a table
<template>
    <td>{{employee.employee_name}}</td>
    <td>${{Number(employee.employee_salary).toLocaleString('en-US')}}</td>
    <td>{{employee.employee_age}}</td>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { IEmployee } from '../types/Employee.interface';

export default defineComponent({
    name: 'Employee',
    props: {
        employee: Object as PropType<IEmployee>
    },
})
</script>

<style scoped>
td {
    padding: 10px 15px;
}
</style>