<template>
    <div class="mt-4">
        <h3>Free Members By Section</h3>
        <table class="table table-striped table-hover">
            <thead>
                <tr>
                    <th>Employee Name</th>
                    <th>This Month</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="employee in paginatedEmployeeData" :key="employee.id">
                    <td>{{ employee.full_name }}</td>
                    <td>{{ employee.scale }}</td>
                </tr>
            </tbody>
        </table>
        <paginate :page-count="totalPages" :page-range="3" :margin-pages="2" :click-handler="handlePageChange"
            :prev-text="'<<<'" :next-text="'>>>'" :container-class="'pagination'" :page-class="'page-item'">
        </paginate>
    </div>
</template>

<script>
import axios from "axios";
import Paginate from "vuejs-paginate-next";

export default {
    components: {
        paginate: Paginate,
    },
    data() {
        return {
            employeeData: [],
            currentPage: 1,
            itemsPerPage: 15,
        };
    },

    computed: {
        totalPages() {
            return Math.ceil(this.employeeData.length / this.itemsPerPage);
        },

        paginatedEmployeeData() {
            const startIndex = (this.currentPage - 1) * this.itemsPerPage;
            const endIndex = startIndex + this.itemsPerPage;
            return this.employeeData.slice(startIndex, endIndex);
        },
    },

    mounted() {
        this.getEmployeeData()
    },

    methods: {
        async getEmployeeData() {
            try {
                const response = await axios.get('http://localhost:8000/api/cv_project');
                this.employeeData = response.data.data;
            } catch (error) {
                console.error('Error fetching employee data:', error);
            }
        },
        handlePageChange(newPage) {
            this.currentPage = newPage;
        },
    },
}
</script>
