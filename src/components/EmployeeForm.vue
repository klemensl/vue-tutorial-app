<template>
  <div id="employee-form">
    <form @submit.prevent="handleSubmit">
      <label>Employee name</label>
		<input
			type="text"
			ref="name"
			:class="{ 'has-error': submitting && invalidName }"
			v-model="employee.name" 
			@focus="clearStatus"
			@keypress="clearStatus"
		/> <!-- wenn man sich hier bei "employee.name" verschreibt bekommt man kein warning?! -->
		<label>Employee Email</label>
		<input
			type="text"
			:class="{ 'has-error': submitting && invalidEmail }"
			v-model="employee.email"
			@focus="clearStatus"
		/>
		<p v-if="error && submitting" class="error-message">
			❗Please fill out all required fields
		</p>
		<p v-if="success" class="success-message">
			✅ Employee successfully added
		</p>
		<button>Add Employee</button>
    </form>
  </div>
</template>

<script>
  export default {
    name: 'employee-form',
    data() {
      return {
		submitting: false,
		error: false,
		success: false,
        employee: {
          name: '',
          email: '',
        },
      }
	},
	methods: {
		handleSubmit() {
			this.submitting = true
			this.success = false
			this.error = false

			if (this.invalidName || this.invalidEmail) {
				this.error = true
				return
			}

			this.$emit('add:employee', this.employee) // emit event zu den parents
			this.$refs.name.focus()

			this.employee = {
				name: '',
				email: '',
			}
			this.error = false
			this.success = true
			this.submitting = false
		},

		clearStatus() {
			this.success = false
			this.error = false
		},
	},
    computed: {
      invalidName() {
        return this.employee.name === ''
      },

      invalidEmail() {
        return this.employee.email === ''
      },
    },
  }
</script>

<style scoped>
  form {
    margin-bottom: 2rem;
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