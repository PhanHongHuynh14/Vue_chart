<template>
    <div class="mt-4">
        <h3>Free Members In Jan 2024</h3>
        <table class="table table-striped table-hover">
            <thead>
                <tr>
                    <th>Name</th>
                    <th>Section</th>
                    <th>Rank</th>
                    <th>Percentage</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="section in paginatedSectionData" :key="section.id">
                    <td>{{ section.full_name }}</td>
                    <td>{{ section.name }}</td>
                    <td>{{ section.rank }}</td>
                    <td>{{ section.effort_percentage }}%</td>
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
            sectionData: [],
            currentPage: 1,
            itemsPerPage: 15,
        }
    },

    computed: {
        totalPages() {
            return Math.ceil(this.sectionData.length / this.itemsPerPage)
        },

        paginatedSectionData() {
            const startIndex = (this.currentPage - 1) * this.itemsPerPage;
            const endIndex = startIndex + this.itemsPerPage

            return this.sectionData.slice(startIndex, endIndex);
        },
    },

    mounted() {
        this.getData()
    },

    methods: {
        async getData() {
            try {
                const response = await axios.get('http://localhost:8000/api/cv_project');
                this.sectionData = response.data.data;
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