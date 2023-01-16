<template>
  <div class="forms">
    <el-card shadow="hover">
      <div slot="header" class="clearfix">
        <span>User registration</span>
      </div>
      <el-form :rules="rules" ref="form" :model="loginForm" label-width="80px">
        <el-form-item label="Username" prop="name">
          <el-input v-model="loginForm.name"></el-input>
        </el-form-item>
        <el-form-item label="Password" prop="password">
          <el-input type="password" v-model="loginForm.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Email" prop="email">
          <el-input v-model="loginForm.email"></el-input>
        </el-form-item>
        <el-form-item label="Occupation" prop="occupation">
          <el-select v-model="loginForm.occupation" placeholder="Please select occupation">
            <el-option v-for="items in form.occupation" :key="items" :label="items" :value="items"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="State" prop="state">
          <el-select v-model="loginForm.state" placeholder="Please select a state">
            <el-option v-for="items in form.state" :key="items.abbreviation" :label="items.name" :value="items.name"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('form')">Submit</el-button>
          <el-button @click="resetForm('form')">Reset</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Forms',
  data() {
    return {
      form: {
        name: '',
        password: '',
        email: '',
        occupation: [],
        state: []
      },
      loginForm: {
        name: '',
        password: '',
        email: '',
        occupation: '',
        state: ''
      },
      rules: {
        name: [
          { required: true, message: 'User name cannot be empty', trigger: 'blur' },
          { min: 3, max: 12, message: '3 to 12 characters in length', trigger: 'blur' }
        ],
        password: [
          { required: true, message: 'Password cannot be empty', trigger: 'blur' },
          { min: 6, max: 12, message: 'Password length is between 6 and 12 characters', trigger: 'blur' }
        ],
        email: [
          { required: true, message: 'The mailbox cannot be empty', trigger: 'blur' },
          { type: 'email', message: 'Please enter the correct mailbox', trigger: 'blur' }
        ],
        occupation: [{ required: true, message: 'Please select occupation', trigger: 'change' }],
        state: [{ required: true, message: 'Please select a state', trigger: 'change' }]
      }
    }
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          //Register successfully to initiate a post request
          axios
            .post('https://frontend-take-home.fetchrewards.com/form', this.loginForm)
            .then((res) => {
              console.log(res)
            })
            .catch(function (error) {
              // Handling error situations
              return console.log('post submit failed' + error)
            })
          this.$message('Register successfully!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
    }
  },
  created() {
    //The page rendering cycle initiates get requests
    axios
      .get('https://frontend-take-home.fetchrewards.com/form')
      .then((res) => {
        console.log(res)
        this.form.occupation = res.data.occupations
        this.form.state = res.data.states
      })
      .catch(function (error) {
        // Handling error situations
        return console.log('The get request failed' + error)
      })
  }
}
</script>

<style scoped lang="less">
.forms {
  width: 850px;
  margin: 60px auto;
}
</style>
