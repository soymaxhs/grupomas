<template>
    <b-container class="my-5">
        <b-form-row inline>
            <b-col>
                <label class="sr-only" for="inline-form-input-url">Ingrese la url del catalogo a consultar</label>
                <b-form-input id="inline-form-input-url" class="mb-2 mr-sm-2 mb-sm-0"
                    placeholder="https://mi-direccion.com"
                    v-model="inputUrl">
                </b-form-input>
            </b-col>
            <b-button variant="primary" class="my-3" @click="getJson()">Consultar</b-button>
        </b-form-row>
        <DataTable v-if="data!=null" :options="{
        select: true,
        buttons: [
            'excel',
            'print',
            'pdf',
            'copy',
        ]
        }" class="display my-5">
            <thead>
                <tr>
                    <th v-for="(item, index) in columns" :key="index">
                        {{item}}
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index) in data" :key="index">
                    <td v-for="(col, i) in columns" :key="i">
                        {{item[col]}}
                    </td>
                </tr>
            </tbody>
        </DataTable>
    </b-container>
</template>
<script>
import DataTable from 'datatables.net-vue3';
import DataTableBs5 from 'datatables.net-bs5';
import Select from 'datatables.net-select';
DataTable.use(DataTableBs5);
DataTable.use(Select);
export default {
    name: 'StartView',
    components: {
        DataTable,
    },
    data() {
        return {
            columns: null,
            data: null,
            inputUrl: 'https://vpic.nhtsa.dot.gov/api/vehicles/decodevin/5UXWX7C5*BA?format=json&modelyear=2011',
        }
    },
    methods: {
        async getJson(){
            this.data = null;
            await this.axios.get(this.inputUrl).then((response) => {
                this.columns = Object.keys(response.data.Results[0]);
                this.data = response.data.Results
            })
        },
    },
}
</script>
<style>
    /* @import 'datatables.net-dt'; */
    @import 'datatables.net-bs5';
</style>