<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <h1 class="text-center">User Information Form</h1>

        <form @submit.prevent="submitForm" novalidate>
          <!-- Username -->
          <div class="row mb-3">
            <div class="col-12 col-md-6">
              <label for="username" class="form-label">Username</label>
              <input
                type="text"
                class="form-control"
                id="username"
                v-model="formData.username"
                @blur="() => validateName(true)"
                @input="() => validateName(false)"
              />
              <div v-if="errors.username" class="text-danger">{{ errors.username }}</div>
            </div>

            <!-- Password -->
            <div class="col-12 col-md-6">
              <label for="password" class="form-label">Password</label>
              <input
                type="password"
                class="form-control"
                id="password"
                v-model="formData.password"
                @blur="() => validatePassword(true)"
                @input="() => validatePassword(false)"
              />
              <div v-if="errors.password" class="text-danger">{{ errors.password }}</div>
            </div>
          </div>

          <!-- Checkbox and Gender -->
          <div class="row mb-3">
            <div class="col-12 col-md-6">
              <div class="form-check mt-4">
                <input
                  type="checkbox"
                  class="form-check-input"
                  id="isAustralian"
                  v-model="formData.isAustralian"
                  @change="validateResident"
                />
                <label class="form-check-label" for="isAustralian">Australian Resident?</label>
                <div v-if="errors.resident" class="text-danger">{{ errors.resident }}</div>
              </div>
            </div>

            <div class="col-12 col-md-6">
              <label for="gender" class="form-label">Gender</label>
              <select
                class="form-select"
                id="gender"
                v-model="formData.gender"
                @change="validateGender"
              >
                <option value="">Select Gender</option>
                <option value="male">Male</option>
                <option value="female">Female</option>
                <option value="other">Other</option>
              </select>
              <div v-if="errors.gender" class="text-danger">{{ errors.gender }}</div>
            </div>
          </div>

          <!-- Reason -->
          <div class="mb-3">
            <label for="reason" class="form-label">Reason for joining</label>
            <textarea
              class="form-control"
              id="reason"
              rows="3"
              v-model="formData.reason"
              @blur="validateReason"
              @input="validateReason"
            ></textarea>
            <div v-if="errors.reason" class="text-danger">{{ errors.reason }}</div>
          </div>

          <!-- Submit -->
          <div class="text-center">
            <button type="submit" class="btn btn-primary me-2">Submit</button>
            <button type="button" class="btn btn-secondary" @click="clearForm">Clear</button>
          </div>
        </form>

        <!-- Cards showing submitted data -->
        <div class="row mt-5" v-if="submittedCards.length">
          <div class="d-flex flex-wrap justify-content-start">
            <div
              v-for="(card, index) in submittedCards"
              :key="index"
              class="card m-2"
              style="width: 18rem"
            >
              <div class="card-header">User Information</div>
              <ul class="list-group list-group-flush">
                <li class="list-group-item">Username: {{ card.username }}</li>
                <li class="list-group-item">Password: {{ card.password }}</li>
                <li class="list-group-item">Australian Resident: {{ card.isAustralian ? 'Yes' : 'No' }}</li>
                <li class="list-group-item">Gender: {{ card.gender }}</li>
                <li class="list-group-item">Reason: {{ card.reason }}</li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 表单字段
const formData = ref({
  username: '',
  password: '',
  isAustralian: false,
  gender: '',
  reason: ''
})

// 错误信息
const errors = ref({
  username: null,
  password: null,
  resident: null,
  gender: null,
  reason: null
})

const submittedCards = ref([])

// Username 验证
const validateName = (blur) => {
  if (formData.value.username.length < 3) {
    if (blur) errors.value.username = 'Name must be at least 3 characters'
  } else {
    errors.value.username = null
  }
}

// Password 验证（含大写、小写、数字、特殊字符）
const validatePassword = (blur) => {
  const password = formData.value.password
  const minLength = 8
  const hasUppercase = /[A-Z]/.test(password)
  const hasLowercase = /[a-z]/.test(password)
  const hasNumber = /\d/.test(password)
  const hasSpecialChar = /[!@#$%^&*(),.?":{}|<>]/.test(password)

  if (
    password.length < minLength ||
    !hasUppercase ||
    !hasLowercase ||
    !hasNumber ||
    !hasSpecialChar
  ) {
    if (blur) {
      errors.value.password = 'Password must be at least 8 characters, include upper and lower case letters, a number, and a special character.'
    }
  } else {
    errors.value.password = null
  }
}

// Checkbox 验证
const validateResident = () => {
  if (!formData.value.isAustralian) {
    errors.value.resident = 'You must confirm residency.'
  } else {
    errors.value.resident = null
  }
}

// Gender 验证
const validateGender = () => {
  if (!formData.value.gender) {
    errors.value.gender = 'Please select a gender.'
  } else {
    errors.value.gender = null
  }
}

// Reason 验证
const validateReason = () => {
  if (!formData.value.reason || formData.value.reason.length < 10) {
    errors.value.reason = 'Reason must be at least 10 characters long.'
  } else {
    errors.value.reason = null
  }
}

// 提交表单时统一验证
const submitForm = () => {
  validateName(true)
  validatePassword(true)
  validateResident()
  validateGender()
  validateReason()

  if (
    !errors.value.username &&
    !errors.value.password &&
    !errors.value.resident &&
    !errors.value.gender &&
    !errors.value.reason
  ) {
    submittedCards.value.push({ ...formData.value })
    clearForm()
  }
}

// 清空表单
const clearForm = () => {
  formData.value = {
    username: '',
    password: '',
    isAustralian: false,
    gender: '',
    reason: ''
  }
  errors.value = {
    username: null,
    password: null,
    resident: null,
    gender: null,
    reason: null
  }
}
</script>

<style scoped>
.card {
  border: 1px solid #ccc;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.card-header {
  background-color: #275fda;
  color: white;
  padding: 10px;
  border-radius: 10px 10px 0 0;
}

.list-group-item {
  padding: 10px;
}
</style>
