<template>
    <div id="employee-table">
        <p v-if="employeesss.length < 1" class="empty-table">
            No employees
        </p>
        <table v-else>
            <thead>
                <tr>
                    <th>Employee Name</th>
                    <th>Employee Email</th>
                    <th>Actions</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="employee in employeesss" v-bind:key="employee.id">
                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.name">
                    </td>
                    <td v-else>{{ employee.name }}</td>
                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.email">
                    </td>
                    <td v-else>{{ employee.email }}</td>
                    <td v-if="editing === employee.id">
                        <button v-on:click="editEmployee(employee)">Save</button>
                        <button v-on:click="cancelEdit(employee)" class="muted-button">Cancel</button>
                    </td>
                    <td v-else>
                        <button v-on:click="editMode(employee)">Edit</button>
                        <button v-on:click="$emit('delete:employee', employee.id)">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'employee-table',
    data() {
        return {
            editing: null,
            cachedEmployee: {},
        }
    },
    props: {
        employeesss: Array,
        // props: ['employeesss'],
    },
    methods: {
        editMode(employee) {
            this.cachedEmployee = Object.assign({}, employee)
            this.editing = employee.id
        },

        cancelEdit(employee) {
            Object.assign(employee, this.cachedEmployee)
            this.editing = null
        },

        editEmployee(employee) {
            if (employee.name === '' || employee.email === '') return
            this.$emit('edit:employee', employee.id, employee)
            this.editing = null
        }
    }
}
</script>

<style scoped>
    button {
        margin: 0 0.5rem 0 0;
    }
</style>
