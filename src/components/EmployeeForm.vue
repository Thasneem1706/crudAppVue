<template>
    <div id="employee-form">
        <!-- <form v-on:submit.prevent = "handleSubmit" action=""> -->
        <form @submit.prevent = "handleSubmit"  action=""> 
            <label for="">Employee Name</label>
            <input type="text"
             v-model="employee.name"
             :class="{'has-error':submitting&&invalidName}"
             @focus="clearStatus"
             @keypress="clearStatus"
             
             ref="firstInput">
            <label for="">Employee Mail</label>
            <input type="text" v-model="employee.email"
            :class="{'has-error':submitting&&invalidMail}"
            @focus="clearStatus"
            @keypress="clearStatus">
            
            <p v-if="submitting && error" class="error-message">please fill out the required details!</p>
            <p v-if="success" class="success-message">Employee successfully added</p>
            <button>Add employee</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'employee-form',
    data(){        
        return{
            submitting:false,
            success:false,
            error:false,
            employee :{
                name:'',
                email:''
            }
        }
    },
    methods: {
        handleSubmit(){
            this.submitting = true;
            this.clearStatus();

            if(this.invalidName || this.invalidMail){
                this.error = true;
                return;
            }

            this.$emit('add:employee',this.employee); //event emitting 
            this.$refs.firstInput.focus() //template reference
            this.employee = {
                name:'',
                email:''
            }
            this.success= true;
            this.error=false;
            this.submitting= false;
        },
        clearStatus(){
            this.success= false,
            this.error= false

        }
    },
    computed:{
        invalidName(){
            return this.employee.name === '';
        },
        invalidMail(){
            return this.employee.email === '';
        }
    }

    
}
</script>

<style scoped>
    form{
        margin-bottom: 2em;
    }
    [class*='-message'] {
        font-weight: 500;
    }

    .error-message {
        color: #d33c40;
    }

    .success-message {
        color: #32a95d;
    }

</style>
