<template>
    <div id="employee-form" class="container">
        <form @submit.prevent="handleSubmit">
            <p>Employee Name: <input type="text" 
                v-model="employee.name" 
                placeholder="Enter Name"
                :class="['form-control', 'w-25',{'border border-danger':submitting&&invalidName}]"
                @focus="clearStatus"
                @keypress="clearStatus"
                ref="firstInput"
            ></p>
            <p>Employee Email: <input type="email" 
                v-model="employee.email" 
                placeholder="Enter Email"
                :class="['form-control', 'w-25',{'border border-danger':submitting&&invalidEmail}]"
                @focus="clearStatus"
                @keypress="clearStatus"
            ></p>

            <p v-if="submitting && error" class="text-danger">Please fill out all the required fields!</p>
            <p v-if="success" class="text-success">Employee successfully added!</p>

            <p><button class="btn btn-success w-25">Add Employee</button></p>
        </form>
    </div>
</template>

<script>
export default {
    name: 'EmployeeForm',
    data() {
        return {
            submitting: false,
            success: false,
            error: false,
            employee: {
                name: '',
                email: '',
            }
        }
    },
    methods: {
        handleSubmit() {
            this.submitting=true;
            this.clearStatus();

            if(this.invalidName||this.invalidEmail){
                this.error=true;
                return;
            }
            
            this.$emit('add:employee', this.employee);
            this.$refs.firstInput.focus();
            this.employee = {
                name: '',
                email: ''
            };
            this.success=true;
            this.error=false;
            this.submitting=false;
        },
        clearStatus(){
            this.success=false;
            this.error=false;
        }
    },
    computed: {
        invalidName() {
            return this.employee.name === '';
        },
        invalidEmail() {
            return this.employee.email === '';
        }
    }
}
</script>

<style scoped></style>