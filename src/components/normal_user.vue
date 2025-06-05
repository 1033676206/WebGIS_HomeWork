<template>
  <div class="login-page">
    <!-- 顶部导航栏 -->
    <div class="top-bar">
      <h1 class="webgis-title">华航WebGIS</h1>
      <!-- <h1极客 class="webgis-title">华航WebGIS</h1> -->
      <router-link to="/" class="back-to-home">返回主界面</router-link>
    </div>
    
    <!-- 登录容器 -->
    <div class="login-container">
      <div class="left-section">
        <!-- 使用安全的图片引用方式 -->
        <img src="../assets/pic/login_showImg.jpg" alt="校园图片" />
        <div class="welcome-text">
          <h2>Welcome to HuaHang WebGIS</h2>
        </div>
      </div>
      <div class="right-section">
        <h2>账号密码登录</h2>
        <form @submit.prevent="login" class="login-form">
          <!-- 添加包裹容器并设置垂直间距 -->
          <div class="form-content">
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
              <div class="password-container">
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
            </div>
          </div>
          
          <div class="error-message" v-if="errorMessage">{{ errorMessage }}</div>
          
          <button type="submit">登录</button>
          
          <div class="bottom-links">
            <div class="left-link">
              <router-link to="/forget">忘记密码？</router-link>
            </div>
            <div class="right-link">
              <router-link to="/register">新用户注册</router-link>
            </div>
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
  background-image: url('https://s1.imagehub.cc/images/2023/10/08/R-C.jpeg');
  background-size: cover;
  background-position: center;
  position: relative;
  padding-top: 60px;
}

/* 顶部导航栏 */
.top-bar {
  background-color: #227700;
  width: 100%;
  padding: 20px;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 100;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  display: flex;
  justify-content: center;
  align-items: center;
}

.webgis-title {
  font-family: "LiSu", "SimSun", serif;
  font-size: 40px;
  color: white;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
  margin: 0 auto;
  text-align: center;
}

.back-to-home {
  color: white;
  text-decoration: none;
  font-size: 16px;
  font-weight: 500;
  position: absolute;
  right: 20px;
  top: 50%;
  transform: translateY(-50%);
  padding: 5px 10px;
  border-radius: 3px;
  background: rgba(255, 255, 255, 0.2);
  transition: background 0.3s;
}

.back-to-home:hover {
  background: rgba(255, 255, 255, 0.3);
}

/* 登录容器 */
.login-container {
  display: flex;
  max-width: 1000px;
  width: 95%;
  margin: 30px auto 20px auto;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  border: 1px solid rgba(0, 0, 0, 0.1);
  min-height: 480px;
  /* 添加垂直居中对齐 */
  align-items: center;
}

/* 主要修改点：左侧部分下移 */
.left-section {
  flex: 0 0 50%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start; /* 从顶部开始而不是居中 */
  align-items: center;
  padding: 40px 25px 25px; /* 增加顶部内边距 */
  background: rgba(34, 119, 0, 0.05);
  margin-top: 30px; /* 添加顶部外边距使整体下移 */
}

.left-section img {
  width: 100%;
  max-height: 300px;
  object-fit: cover;
  border-radius: 5px;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

.welcome-text {
  text-align: center;
  margin-top: auto; /* 使欢迎文本在左侧区域底部 */
}

.welcome-text h2 {
  font-size: 22px;
  color: #333;
  margin: 0;
  font-weight: 600;
}

/* 右侧部分微调使其与左侧高度匹配 */
.right-section {
  flex: 0 0 50%;
  padding: 30px;
  display: flex;
  flex-direction: column;
  justify-content: center; /* 保持垂直居中 */
  align-items: center;
}

.right-section h2 {
  text-align: center;
  color: #227700;
  margin-bottom: 20px;
  font-size: 24px;
  font-weight: bold;
}

/* 表单容器 */
.login-form {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* 添加的包裹容器 - 用于调整输入框位置 */
.form-content {
  margin-top: 20px; /* 使输入框整体下移 */
  margin-bottom: 15px;
  width: 100%;
}

/* 输入框样式 */
.form-row {
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  width: 100%;
  justify-content: center; /* 使输入框居中靠拢 */
}

.form-row label {
  width: 60px; 
  text-align: right; 
  padding-right: 10px; 
  color: #555;
  font-weight: 500;
  font-size: 15px;
}

/* 密码容器 */
.password-container {
  position: relative;
  display: inline-block;
}

input {
  width: 200px;
  height: 35px;
  padding: 0 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 15px;
  transition: all 0.2s ease;
}

input:focus {
  outline: none;
  border-color: #3a9d5c;
  box-shadow: 0 0 0 2px rgba(58, 157, 92, 0.2);
}

/* 密码可见性切换 */
.password-toggle {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
  color: #777;
  cursor: pointer;
  font-size: 16px;
}

.error-message {
  color: #极客 #e74c3c;
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
  width: 200px;
  height: 40px;
  background: #227700;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16极客 px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 10px;
}

button:hover {
  background: #1a6600;
}

/* 底部链接 - 均匀分布在登录按钮下方左右两侧 */
.bottom-links {
  width: 200px;
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}

.left-link, .right-link {
  width: 45%;
  text-align: center;
}

.left-link {
  text-align: left;
}

.right-link {
  text-align: right;
}

.bottom-links a {
  color: #227700;
  text-decoration: none;
  font-size: 13px;
  font-weight: 500;
  transition: all 0.3s ease;
}

.bottom-links a:hover {
  text-decoration: underline;
}

.footer {
  text-align: center;
  padding: 15px;
  color: white;
  font-size: 13px;
  background: rgba(0, 0, 0, 0.6);
  margin-top: auto;
  width: 100%;
}

.footer a {
  color: white;
  text-decoration: none;
  font-weight: 500;
}

.footer a:hover {
  text-decoration: underline;
}

/* 响应式设计 */
@media (max-width: 900px) {
  .login-container {
    flex-direction: column;
    margin: 20px auto;
  }
  
  .left-section, .right-section {
    padding: 20px;
    width: 100%;
  }
  
  .left-section {
    margin-top: 0; /* 响应式中取消顶部外边距 */
    padding-top: 30px;
  }
  
  .left-section img {
    max-width: 250px;
    max-height: 200px;
  }
  
  .right-section {
    padding-top: 10px;
  }
  
  .form-row, input, button, .bottom-links {
    width: 100%;
  }
  
  .password-container {
    width: 100%;
  }
}

@media (max-width: 600px) {
  .top-bar {
    padding: 15px 10px;
  }
  
  .webgis-title {
    font-size: 32px;
  }
  
  .back-to-home {
    font-size: 14px;
    padding: 3px 8px;
    right: 10px;
  }
  
  input {
    width: 100%;
  }
  
  .form-row label {
    width: 50px;
    text-align: left;
  }
  
  .bottom-links {
    flex-direction: column;
  }
  
  .left-link, .right-link {
    width: 100%;
    text-align: center;
    margin: 5px 0;
  }
}
</style>