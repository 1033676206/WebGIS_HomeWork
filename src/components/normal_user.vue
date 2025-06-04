<template>
  <div class="login-page">
    <!-- 顶部导航栏 - 使用第一张图的样式 -->
    <div class="top-bar">
      <div class="top-inner">
        <h1 class="webgis-title">华航WebGIS</h1>
        <router-link to="/" class="back-to-home">返回主界面</router-link>
      </div>
    </div>
    
    <!-- 登录容器 - 减少边距，使用第一张图的底图 -->
    <div class="login-container">
      <div class="left-section">
        <img src="https://s1.imagehub.cc/images/2023/10/08/100w0t000000ivz6p83BE.jpeg" alt="校园图片" />
        <div class="welcome-text">
          <h2>Welcome to HuaHang WebGIS</h2>
        </div>
      </div>
      <div class="right-section">
        <h2>账号密码登录</h2>
        <form @submit.prevent="login">
          <!-- 修改输入框结构，解决标签重叠问题 -->
          <div class="form-row">
            <label for="username">用户:</label>
            <input 
              type="text" 
              id="username" 
              v-model.trim="username" 
              required
              placeholder="输入账号"
            />
          </div>
          
          <div class="form-row">
            <label for="password">密码:</label>
            <input 
              type="password" 
              id="password" 
              v-model.trim="password" 
              required
              placeholder="输入密码"
            />
            <span class="password-toggle" @click="togglePasswordVisibility">
              <i :class="showPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
            </span>
          </div>
          
          <div class="error-message" v-if="errorMessage">{{ errorMessage }}</div>
          
          <button type="submit">登录</button>
          
          <div class="bottom-links">
            <p><router-link to="/forget">忘记密码？</router-link></p>
            <p><router-link to="/register">新用户注册</router-link></p>
          </div>
        </form>
      </div>
    </div>
    
    <div class="footer">
      <p>copyright@2025
        <a href="#">北华航天工业学院</a>
        版权所有 | 地址：河北省廊坊市爱民东道133号 | 邮箱：htzhshb@126.com
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NormalUser',
  data() {
    return {
      username: '',
      password: '',
      showPassword: false,
      errorMessage: ''
    }
  },
  methods: {
    login() {
      // 账号为root，密码为123456
      if (this.username === 'root' && this.password === '123456') {
        // 登录成功处理
        this.errorMessage = ''
        
        // 模拟登录状态
        localStorage.setItem('isLoggedIn', 'true')
        
        // 跳转到主界面
        this.$router.push('/')
      } else {
        // 登录失败处理
        this.errorMessage = '用户名或密码错误！请重试'
        
        // 添加错误样式动画
        const usernameInput = document.getElementById('username')
        const passwordInput = document.getElementById('password')
        
        usernameInput.classList.add('shake')
        passwordInput.classList.add('shake')
        
        setTimeout(() => {
          usernameInput.classList.remove('shake')
          passwordInput.classList.remove('shake')
        }, 500)
      }
    },
    togglePasswordVisibility() {
      this.showPassword = !this.showPassword
      const passwordInput = document.getElementById('password')
      passwordInput.type = this.showPassword ? 'text' : 'password'
    }
  }
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Segoe UI', 'Microsoft YaHei', sans-serif;
}

.login-page {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  /* 使用第一张图的背景 */
  background-image: url('https://s1.imagehub.cc/images/2023/10/08/R-C.jpeg');
  background-size: cover;
  background-position: center;
  position: relative;
}

/* 顶部导航栏 - 使用第一张图的样式 */
.top-bar {
  background-color: #227700; /* 更接近第一张图的绿色 */
  width: 100%;
  padding: 15px 0;
}

.top-inner {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  position: relative;
}

.webgis-title {
  font-family: "LiSu", "SimSun", serif;
  font-size: 40px;
  color: white;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
  /* 居中显示，不留白 */
  width: 100%;
  text-align: center;
  position: absolute;
  left: 0;
}

.back-to-home {
  color: white;
  text-decoration: none;
  font-size: 16px;
  font-weight: 500;
  padding: 8px 15px;
  border-radius: 4px;
  z-index: 10; /* 确保在标题上方 */
}

/* 减少整体边距 */
.login-container {
  display: flex;
  margin: 50px auto; /* 大幅减少上下边距 */
  width: 900px;
  max-width: 95%;
  height: auto;
  min-height: 500px;
  background-color: rgba(255, 255, 255, 0.95); /* 半透明白色 */
  border-radius: 8px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  overflow: hidden;
  border: 1px solid rgba(0, 0, 0, 0.1);
}

.left-section {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 30px;
  background: rgba(34, 119, 0, 0.1); /* 微弱的绿色背景 */
}

.left-section img {
  width: 100%;
  max-width: 280px;
  height: auto;
  border-radius: 8px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

.welcome-text {
  text-align: center;
}

.welcome-text h2 {
  font-size: 24px;
  color: #333;
  margin-bottom: 10px;
}

.right-section {
  flex: 1;
  padding: 30px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.right-section h2 {
  text-align: center;
  color: #227700;
  margin-bottom: 25px;
  font-size: 26px;
}

/* 输入框样式调整 - 解决重叠问题 */
.form-row {
  position: relative;
  margin-bottom: 20px;
  display: flex;
  align-items: center;
}

.form-row label {
  display: block;
  width: 70px; /* 固定标签宽度 */
  color: #555;
  font-weight: 500;
  font-size: 16px;
}

input {
  flex: 1;
  height: 45px;
  padding: 0 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
  transition: all 0.3s ease;
}

input:focus {
  outline: none;
  border-color: #3a9d5c;
  box-shadow: 0 0 0 2px rgba(58, 157, 92, 0.2);
}

/* 密码可见性切换 */
.password-toggle {
  position: absolute;
  right: 15px;
  top: 50%;
  transform: translateY(-50%);
  color: #777;
  cursor: pointer;
  font-size: 16px;
}

.error-message {
  color: #e74c3c;
  font-size: 14px;
  margin: -10px 0 15px 0;
  text-align: center;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  20%, 60% { transform: translateX(-5px); }
  40%, 80% { transform: translateX(5px); }
}

.shake {
  animation: shake 0.5s ease;
  border-color: #e74c3c !important;
}

button {
  width: 100%;
  height: 45px;
  background: #227700;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 10px;
}

button:hover {
  background: #1a6600;
}

.bottom-links {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.bottom-links a {
  color: #227700;
  text-decoration: none;
  font-size: 14px;
  font-weight: 500;
  transition: all 0.3s ease;
}

.bottom-links a:hover {
  text-decoration: underline;
}

.footer {
  text-align: center;
  padding: 20px;
  color: white;
  font-size: 14px;
  background: rgba(0, 0, 0, 0.5);
  margin-top: auto;
}

.footer a {
  color: white;
  text-decoration: none;
  font-weight: 600;
}

.footer a:hover {
  text-decoration: underline;
}

/* 响应式设计 */
@media (max-width: 950px) {
  .login-container {
    width: 95%;
    max-width: 700px;
    flex-direction: column;
    margin: 30px auto;
  }
  
  .left-section, .right-section {
    padding: 30px 20px;
  }
  
  .left-section img {
    max-width: 200px;
  }
}

@media (max-width: 600px) {
  .top-inner {
    flex-direction: column;
    padding: 10px;
  }
  
  .webgis-title {
    position: static;
    margin-bottom: 10px;
    font-size: 32px;
  }
  
  .login-container {
    flex-direction: column;
    margin: 20px auto;
  }
  
  .form-row {
    flex-direction: column;
    align-items: flex-start;
  }
  
  .form-row label {
    margin-bottom: 5px;
  }
  
  input {
    width: 100%;
  }
}
</style>