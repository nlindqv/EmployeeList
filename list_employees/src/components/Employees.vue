// Component to present employees as a table
<template>
<div>
    <table class="table">
        <thead>
            <tr>
                <th @click="$emit('sort', 'employee_name')"
                    v-bind:class="[sortBy === 'employee_name' ? sortDirection : '']">Name</th>
                <th @click="$emit('sort', 'employee_salary')"
                    v-bind:class="[sortBy === 'employee_salary' ? sortDirection : '']">Salary</th>
                <th @click="$emit('sort', 'employee_age')"
                    v-bind:class="[sortBy === 'employee_age' ? sortDirection : '']">Age</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="employee in employees" :key="employee.id">
                <Employee :employee="employee"/>
            </tr>
        </tbody>
    </table>
</div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { IEmployee } from '../types/Employee.interface';
import Employee from './Employee.vue';

export default defineComponent({
    name: 'Employees',
    props: {
        employees: Array as PropType<Array<IEmployee>>,
        sortBy: String,
        sortDirection: String,
    },
    components: {
        Employee,
    },
})
</script>

<style scoped>

.table {
    border-collapse: collapse;
    margin: 20px 0px;
    min-width: 438px;
    border-radius: 5px 5px 0 0;
    overflow: hidden;
}

.table thead tr {
    background-color:#00c281;
    color: #ffffff;
    text-align: left;
    font-weight: bold;
}

.table th{
    padding: 12px 15px;
}

.table tbody tr {
    border-bottom: 1px solid #dddddd;;
}

.table tbody tr:nth-of-type(even) {
    background-color: #f3f3f3;
}

.table tbody tr:last-of-type {
    border-bottom: 2px solid #00c281;
}

.asc:after {
    content: "\25B2";
}

.desc:after {
    content: "\25BC";
}

</style>