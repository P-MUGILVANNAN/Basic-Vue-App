<template>
    <div class="container mt-5">
        <p v-if="employees.length<1" class="">No employees</p>
        <table v-else class="table table-bordered">
            <thead>
                <tr class="text-center">
                    <th>Employee Name</th>
                    <th>Employee Email</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="employee in employees" :key="employee.id" class="text-center">
                    <td v-if="editing==employee.id">
                        <input class="form-control w-100" type="text" v-model="employee.name">
                    </td>
                    <td v-else>{{employee.name}}</td>
                    <td v-if="editing==employee.id">
                        <input class="form-control w-100" type="text" v-model="employee.email">
                    </td>
                    <td v-else>{{employee.email}}</td>
                    <td v-if="editing==employee.id">
                        <button @click="editEmployee(employee)" class="btn btn-success">Save</button>
                        <button @click="cancelEdit(employee)" class="btn btn-danger ms-3">Cancel</button>
                    </td>
                    <td v-else>
                        <button @click="editMode(employee)" class="btn btn-primary">Edit</button>
                        <button @click="$emit('delete:employee',employee.id)" class="btn btn-danger ms-3">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
export default {
    name: 'Home',
    props:{
        employees:Array
    },
    data(){
        return{
            editing:null,
            employee:{
                name:'',
                email:''
            },
            cachedEmployee:null
        }
    },
    methods:{
        editMode(employee){
            this.editing = employee.id;
            this.cachedEmployee = Object.assign({},employee);
        },
        editEmployee(employee){
            if(employee.name==''||employee.email==''){
                return;
            }
            this.$emit('edit:employee',employee.id,employee);
            this.editing=null;
        },
        cancelEdit(employee){
            this.editing=null;
            Object.assign(employee,this.cachedEmployee);
        }
    }
}
</script>
<style>
    
</style>