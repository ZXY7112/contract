<template>
  <div class="register-container">
    <div class="register-card">
      <h3>用户注册</h3>
      
      <div class="form-group">
        <label for="username">用户名</label>
        <input 
          type="text" 
          id="username" 
          v-model="form.username" 
          placeholder="请输入用户名"
        >
        <div v-if="errors.username" class="error-message">{{ errors.username }}</div>
      </div>
      
      <div class="form-group">
        <label for="password">密码</label>
        <input 
          type="password" 
          id="password" 
          v-model="form.password" 
          placeholder="请输入密码"
        >
        <div v-if="errors.password" class="error-message">{{ errors.password }}</div>
      </div>
      
      <div class="form-group">
        <label for="confirmPassword">确认密码</label>
        <input 
          type="password" 
          id="confirmPassword" 
          v-model="form.confirmPassword" 
          placeholder="请再次输入密码"
        >
        <div v-if="errors.confirmPassword" class="error-message">{{ errors.confirmPassword }}</div>
      </div>
      
      <!-- 身份选择下拉框 -->
      <div class="form-group">
        <label for="role">用户身份</label>
        <select id="role" v-model="form.role">
          <option value="">请选择身份</option>
          <option value="0">管理员</option>
          <option value="1">起草员</option>
          <option value="2">客户</option>
        </select>
        <div v-if="errors.role" class="error-message">{{ errors.role }}</div>
      </div>
      
      <button class="submit-button" @click="handleSubmit">注册</button>
      
      <div class="login-link">
        已有账号？<router-link to="/login">立即登录</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        username: '',
        password: '',
        confirmPassword: '',
        role: '' // 默认不选择，必须用户手动选择
      },
      errors: {
        username: '',
        password: '',
        confirmPassword: '',
        role: ''
      }
    }
  },
  methods: {
    validateForm() {
      let isValid = true;
      this.errors = {};
      
      if (!this.form.username.trim()) {
        this.errors.username = '用户名不能为空';
        isValid = false;
      }
      
      if (!this.form.password) {
        this.errors.password = '密码不能为空';
        isValid = false;
      } else if (this.form.password.length < 6) {
        this.errors.password = '密码长度至少6位';
        isValid = false;
      }
      
      if (!this.form.confirmPassword) {
        this.errors.confirmPassword = '请确认密码';
        isValid = false;
      } else if (this.form.confirmPassword !== this.form.password) {
        this.errors.confirmPassword = '两次输入的密码不一致';
        isValid = false;
      }
      
      if (this.form.role === '') {
        this.errors.role = '请选择用户身份';
        isValid = false;
      }
      
      return isValid;
    },
    
    async handleSubmit() {
      if (!this.validateForm()) return;
      
      try {
        // 发送注册请求到后端
        await this.$axios.post('/api/register', this.form);
        
        // 注册成功提示
        alert('注册成功！请登录');
        this.$router.push('/login');
      } catch (error) {
        console.error('注册失败:', error);
        
        // 显示服务器返回的错误信息
        if (error.response && error.response.data.message) {
          alert(error.response.data.message);
        } else {
          alert('注册失败，请稍后重试');
        }
      }
    }
  }
}
</script>

<style scoped>
.register-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f5f7fa;
}

.register-card {
  width: 100%;
  max-width: 400px;
  padding: 30px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

h3 {
  margin-top: 0;
  margin-bottom: 25px;
  text-align: center;
  color: #333;
}

.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #555;
}

input, select {
  width: 100%;
  padding: 10px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  transition: border-color 0.2s;
}

input:focus, select:focus {
  outline: none;
  border-color: #2c3e50;
  box-shadow: 0 0 0 2px rgba(64, 158, 255, 0.2);
}

.error-message {
  color: #f56c6c;
  font-size: 12px;
  margin-top: 5px;
}

.submit-button {
  width: 100%;
  padding: 12px;
  background-color: #2c3e50;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.submit-button:hover {
  background-color: #2c3e50;
}

.login-link {
  margin-top: 15px;
  text-align: center;
  font-size: 14px;
  color: #666;
}

.login-link a {
  color: #409eff;
  text-decoration: none;
}

.login-link a:hover {
  text-decoration: underline;
}
</style>