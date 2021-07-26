<template>
<div class="container">
  <Header @toggle-filter="toggleFilter"
          title="List of Employees"
          :showFilter="showFilter"/>
  <div v-if="showFilter">
    <Filter @set-age-range="filterAgeRange"
            @set-salary-range="filterSalaryRange"
            :ageRange="getAgeRange"
            :salaryRange="getSalaryRange"/>
  </div>

  <Employees @sort="sort"
          :employees="sortedFilteredEmployees"
          :sortBy="sortBy"
          :sortDirection="sortDirection"/>
</div>

</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Header from './components/Header.vue';
import Employees from './components/Employees.vue';
import Filter from './components/Filter.vue';
import { IEmployee } from './types/Employee.interface'

export default defineComponent({
  name: 'App',
  components: {
    Header,
    Employees,
    Filter
  },
  data() {
      return {
          showFilter: false,
          employees: [] as IEmployee[], /** List of employees */
          sortBy: "employee_name",      /** Attribute to sort employees by*/
          sortDirection: "asc",         /** Sorting order of employees*/
          ageRange: [] as number[],     /** min and max ages among the employees*/
          salaryRange: [] as number[],  /** min and max salary among the employees*/
          filteredAgeRange: [] as number[], /** min and max ages to filter the employees by*/
          filteredSalaryRange: [] as number[], /** min and max salary to filter the employees by*/
      }
  },

  methods: {
    /** Fetch employee data using rest api, if no valid response is given, wait and try again */
    async fetchEmployees(): Promise<IEmployee[]> {
      let res = await fetch('http://dummy.restapiexample.com/api/v1/employees')
      while (!res.ok) {
        console.log('Failed to get a vaild response, will try again in 2 seconds...')
        await new Promise(r => setTimeout(r, 2000)); // Sleep 2 sec then retry
        res = await fetch('http://dummy.restapiexample.com/api/v1/employees');
      }
      const data = await res.json()
      return data.data
    },

    /** Toggle filter sliders */
    toggleFilter(): void {
      this.showFilter = !this.showFilter;
    },
    /** Set sorting criteria */
    sort (s: string): void{
      if (s === this.sortBy) {
        this.sortDirection = this.sortDirection === 'asc' ? 'desc' : 'asc';
      }
      this.sortBy = s;
    },

    /** Set filtering ranges */
    filterAgeRange (newRange: number[]): void {
      this.filteredAgeRange = newRange;
    },
    filterSalaryRange (newRange: number[]): void {
      this.filteredSalaryRange = newRange;
    },

  },

  computed: {
    /** Sort and filter employees before rendering*/
    sortedFilteredEmployees(): IEmployee[] {
      return [...this.employees].sort((a,b) => {
                    let modifier = 1;
                    if(this.sortDirection === 'desc') modifier = -1;
                    if((a[this.sortBy as keyof IEmployee] || a.id) < (b[this.sortBy as keyof IEmployee] || b.id)) return -1 * modifier;
                    if((a[this.sortBy as keyof IEmployee] || a.id) > (b[this.sortBy as keyof IEmployee] || b.id)) return 1 * modifier;
                    return 0;
                }).filter(employee => employee.employee_age >= this.filteredAgeRange[0] && employee.employee_age <= this.filteredAgeRange[1]
                ).filter(employee => employee.employee_salary >= this.filteredSalaryRange[0] && employee.employee_salary <= this.filteredSalaryRange[1]);
    },

    getAgeRange(): number[] {
      const min = Math.min(...this.employees.map(function(employee: IEmployee) { return employee.employee_age;}));
      const max = Math.max(...this.employees.map(function(employee: IEmployee) { return employee.employee_age;}));
      return [min, max];
    },
    getSalaryRange(): number[] {
      const min = Math.min(...this.employees.map(function(employee: IEmployee) { return employee.employee_salary;}));
      const max = Math.max(...this.employees.map(function(employee: IEmployee) { return employee.employee_salary;}));
      return [min, max];
    },
  },

  async created(): Promise<void> {
    this.employees = await this.fetchEmployees()
    this.filteredSalaryRange = this.getSalaryRange
    this.filteredAgeRange = this.getAgeRange
  },
});
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
  color: #3c3c3c;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid #3c3c3c;
  padding: 30px;
  border-radius: 5px;
}

.filter {
    padding: 0 25px;
    border: 3px solid #dddddd;
    background-color: #f3f3f3;
    border-radius: 7px;

}
.slider {
    margin: 30px 0;
}

.btn {
  display: inline-block;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 0px;
  border-radius: 7px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}

</style>
