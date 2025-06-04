<template>
  <div class="page-container">
    <!-- 导航栏 - 关键修复登录按钮区域 -->
    <nav class="navbar">
      <div class="logo">
        <h1 class="webgis-title">华航WebGIS</h1>
      </div>
      <ul class="nav-links">
        <li><router-link to="/location">学校历史</router-link></li>
        <li><router-link to="/complex">建制变迁</router-link></li>
        <li><router-link to="/history">校史文化标注</router-link></li>
        <li><router-link to="/schoolmate">知名校友</router-link></li>
        <li><router-link to="/feedback">用户反馈</router-link></li>
      </ul>
      <div class="login-wrapper">
        <button 
          class="login-button" 
          @mouseenter="showLoginOptions = true" 
          @mouseleave="delayHideOptions"
        >
          <span>登录</span>
          <span class="circle"></span>
          <span class="arrow-icon" :class="{ up: showLoginOptions }"></span>
        </button>

        <div 
          class="user-options-container"
          v-show="showLoginOptions"
          @mouseenter="showLoginOptions = true"
          @mouseleave="hideOptionsNow"
        >
          <ul class="user-options">
            <!-- 修改这里：添加路由跳转 -->
            <li @click="$router.push('/normaluser')">普通用户</li>
            <li @click="loginAs('admin')">管理员</li>
          </ul>
          
          <!-- 连接区域 - 确保鼠标可以移动过去 -->
          <div class="menu-connector"></div>
        </div>
      </div>
    </nav>

    <!-- 轮播图区域 (保持原样) -->
    <div class="slider-container" :style="{ height: sliderHeight + 'px' }">
      <div class="slider" @mouseenter="stopAutoPlay" @mouseleave="startAutoPlay">
        <div class="slides" :style="{ transform: `translateX(${-currentSlide * 100}%)` }">
          <div v-for="(slide, index) in slides" :key="index" class="slide-container">
            <div class="slide-content">
              <img 
                :src="slide.image" 
                class="slide-image"
                :alt="'华航历史图片' + (index + 1)"
                :style="{ height: sliderHeight + 'px' }"
              >
              <div class="description-box">
                <p class="description-text">{{ slide.text }}</p>
              </div>
            </div>
          </div>
        </div>
        <div class="slider-dots">
          <span 
            v-for="(dot, i) in slideCount" 
            :key="i"
            :class="{ active: currentSlide === i }"
            @click="goToSlide(i)"
          ></span>
        </div>
        <div class="slider-arrows">
          <span class="arrow left-arrow" @click="prevSlide">&#10094;</span>
          <span class="arrow right-arrow" @click="nextSlide">&#10095;</span>
        </div>
      </div>
    </div>

    <!-- 底部版权信息 -->
    <footer class="footer">
      <p>
        copyright@2025
        <a href="https://www.nuc.edu.cn/">北华航天工业学院</a>
        版权所有
      </p>
      <p class="address">地址：河北省廊坊市爱民东道133号 邮箱：htzhshb@126.com</p>
    </footer>
  </div>
</template>

<script>
import main1 from '@/assets/pic/main_1.png';
import main2 from '@/assets/pic/main_2.png';
import main3 from '@/assets/pic/main_3.png';
import main4 from '@/assets/pic/main_4.png';
import main5 from '@/assets/pic/main_5.png';
import main6 from '@/assets/pic/main_6.png';

export default {
  name: 'MainPage',
  data() {
    return {
      showLoginOptions: false,
      hideOptionsTimeout: null,
      currentSlide: 0,
      autoPlayInterval: null,
      sliderHeight: 0,
      slideCount: 6,
      slides: [
        {
          image: main1,
          text: "华北航天工业学院早期奠基时刻，学院创始人共同描绘教育发展蓝图，在简陋办公室点燃航天教育梦想之火。"
        },
        {
          image: main2,
          text: "2005年共建协议签字仪式隆重举行，河北省政府领导与中国航天科技集团公司代表共同签署战略合作协议。"
        },
        {
          image: main3,
          text: "学院首个航天工程实验室落成典礼，标志着华北航天工业学院进入教学科研新阶段。"
        },
        {
          image: main4,
          text: "2008年新校区开工建设，学校领导亲自奠基，规划现代化教学区与实验室。"
        },
        {
          image: main5,
          text: "2010年学院庆祝第一批毕业生成功进入航天科技集团工作，培养的人才获得行业认可。"
        },
        {
          image: main6,
          text: "2020年学校成立航天科技研究中心，多位院士出席揭牌仪式，为航天教育注入新动力。"
        }
      ]
    };
  },
  mounted() {
    this.calculateSliderHeight();
    this.startAutoPlay();
    window.addEventListener('resize', this.calculateSliderHeight);
  },
  beforeUnmount() {
    this.stopAutoPlay();
    window.removeEventListener('resize', this.calculateSliderHeight);
    if (this.hideOptionsTimeout) clearTimeout(this.hideOptionsTimeout);
  },
  methods: {
    // 登录按钮专用方法
    delayHideOptions() {
      // 延迟300ms关闭，让鼠标有时间移动到菜单
      this.hideOptionsTimeout = setTimeout(() => {
        this.showLoginOptions = false;
      }, 300);
    },
    
    hideOptionsNow() {
      if (this.hideOptionsTimeout) clearTimeout(this.hideOptionsTimeout);
      this.showLoginOptions = false;
    },
    
    cancelHideOptions() {
      if (this.hideOptionsTimeout) clearTimeout(this.hideOptionsTimeout);
    },
    
    // 页面通用方法
    calculateSliderHeight() {
      const navbar = document.querySelector('.navbar');
      const footer = document.querySelector('.footer');
      
      if (navbar && footer) {
        const navbarHeight = navbar.offsetHeight;
        const footerHeight = footer.offsetHeight;
        const windowHeight = window.innerHeight;
        
        this.sliderHeight = Math.max(
          windowHeight - navbarHeight - footerHeight, 
          500
        );
      } else {
        this.sliderHeight = window.innerHeight * 0.8;
      }
    },
    startAutoPlay() {
      this.stopAutoPlay();
      this.autoPlayInterval = setInterval(() => {
        this.nextSlide();
      }, 5000);
    },
    stopAutoPlay() {
      if (this.autoPlayInterval) {
        clearInterval(this.autoPlayInterval);
        this.autoPlayInterval = null;
      }
    },
    nextSlide() {
      this.currentSlide = (this.currentSlide + 1) % this.slideCount;
    },
    prevSlide() {
      this.currentSlide = (this.currentSlide - 1 + this.slideCount) % this.slideCount;
    },
    goToSlide(index) {
      this.currentSlide = index;
    },
    loginAs(role) {
      this.showLoginOptions = false;
      console.log("登录为:", role);
      alert(`您选择了${role === 'admin' ? '管理员' : '普通用户'}登录`);
    }
  }
};
</script>

<style scoped>
/* 全局重置 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  height: 100%;
  width: 100%;
  overflow-x: hidden;
  font-family: 'Microsoft YaHei', sans-serif;
  background-color: #f0f0f0;
}

.page-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* 导航栏 - 关键修复部分 */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
  background-color: #227700;
  color: #ffffff;
  position: sticky;
  top: 0;
  z-index: 1000;
  width: 100%;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.15);
}

.logo {
  display: flex;
  align-items: center;
}

.navbar ul {
  display: flex;
  list-style: none;
  margin: 0 15px;
  flex-wrap: wrap;
  justify-content: center;
}

.navbar li {
  margin: 0 15px;
}

.navbar li a {
  color: #ffffff;
  text-decoration: none;
  font-size: 16px;
  font-weight: 500;
  padding: 8px 10px;
  border-radius: 4px;
  transition: all 0.3s ease;
}

.navbar li a:hover, .navbar li a.router-link-exact-active {
  background-color: rgba(255, 255, 255, 0.15);
}

/* 登录按钮样式修改 - 重要修复部分 */
.login-wrapper {
  position: relative;
  z-index: 1;
}

.login-button {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 8px 16px;
  background-color: #fff;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
  border-radius: 24px;
  border: none;
  cursor: pointer;
  font-size: 15px;
  font-weight: 500;
  color: #333;
  white-space: nowrap;
  transition: all 0.3s ease;
}

.login-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.login-button span:first-child {
  margin-right: 8px;
}

.circle {
  display: inline-block;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: #ccc;
  margin-right: 8px;
}

.arrow-icon {
  position: absolute;
  top: calc(50% - 3px);
  right: 12px;
  width: 0;
  height: 0;
  border: solid #999;
  border-width: 0 2px 2px 0;
  display: inline-block;
  padding: 3px;
  transition: transform 0.3s ease-in-out;
  transform: rotate(45deg);
}

.arrow-icon.up {
  transform: rotate(-135deg);
}

/* 下拉菜单容器 - 修复鼠标无法移动问题 */
.user-options-container {
  position: absolute;
  top: 100%;
  right: 0;
  padding-top: 8px; /* 与按钮的间隙 */
  z-index: 1001;
}

/* 完全重写下拉菜单样式 - 按照图片显示方式 */
.user-options {
  width: 140px;
  background-color: #fff;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  border-radius: 6px;
  overflow: hidden;
}

.user-options li {
  cursor: pointer;
  text-align: center;
  padding: 12px 15px;
  color: #333;
  font-size: 15px;
  transition: all 0.2s ease;
  border-bottom: 1px solid #f0f0f0;
  display: block; /* 确保每行一个选项 */
  white-space: nowrap; /* 防止文字换行 */
}

.user-options li:last-child {
  border-bottom: none;
}

.user-options li:hover {
  background-color: #f5f8fa;
  color: #227700;
}

/* 连接区域 - 解决鼠标移出问题 */
.menu-connector {
  position: absolute;
  top: -8px; /* 与按钮重叠区域 */
  right: 0;
  width: 100%;
  height: 16px; /* 增加重叠区域高度 */
  background-color: transparent;
  z-index: 1002;
}

/* 轮播图容器 */
.slider-container {
  position: relative;
  width: 100%;
  background-color: #e0e0e0;
  overflow: hidden;
}

.slider {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.slides {
  display: flex;
  width: 100%;
  height: 100%;
  transition: transform 0.5s ease;
}

.slide-container {
  min-width: 100%;
  position: relative;
}

.slide-content {
  position: relative;
  width: 100%;
  height: 100%;
}

/* 确保图片显示正常 */
.slide-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  display: block;
  background-color: #2c3e50;
}

/* 优化描述框位置 */
.description-box {
  position: absolute;
  top: 65%; 
  left: 50%;
  transform: translateX(-50%);
  width: 80%;
  max-width: 800px;
  padding: 20px 30px;
  background-color: rgba(70, 70, 70, 0.85);
  border-radius: 12px;
  text-align: center;
  z-index: 2;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(5px);
}

.description-text {
  color: #fff;
  font-size: 20px;
  line-height: 1.6;
  margin: 0;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.4);
  font-weight: 400;
  letter-spacing: 0.5px;
}

.slider-dots {
  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  width: fit-content;
  z-index: 10;
}

.slider-dots span {
  display: block;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.6);
  margin-right: 15px;
  cursor: pointer;
  transition: all 0.3s ease;
  border: 2px solid rgba(255, 255, 255, 0.6);
}

.slider-dots span:hover {
  background-color: rgba(255, 255, 255, 0.9);
  transform: scale(1.1);
}

.slider-dots span.active {
  background-color: #227700;
  border-color: rgba(255, 255, 255, 0.9);
  transform: scale(1.1);
}

.slider-arrows {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  justify-content: space-between;
  width: 100%;
  z-index: 1;
  pointer-events: none;
}

.arrow {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 55px;
  height: 55px;
  background-color: rgba(0, 0, 0, 0.4);
  color: #fff;
  font-size: 30px;
  font-weight: bold;
  cursor: pointer;
  pointer-events: auto;
  user-select: none;
  transition: all 0.3s ease;
  border-radius: 50%;
  margin: 0 25px;
}

.arrow:hover {
  background-color: rgba(0, 0, 0, 0.6);
  transform: scale(1.1);
}

/* 底部版权信息 */
.footer {
  background-color: #f8f8f8;
  padding: 15px 1rem;
  text-align: center;
  flex-shrink: 0;
  font-size: 14px;
  border-top: 1px solid #e0e0e0;
  z-index: 10;
}

.footer a {
  color: #227700;
  text-decoration: none;
  font-weight: 600;
  transition: all 0.2s ease;
}

.footer a:hover {
  color: #1a6300;
  text-decoration: underline;
}

.footer p {
  margin: 0 0 5px 0;
  padding: 0;
  color: #555;
  line-height: 1.4;
}

.address {
  font-weight: normal;
  font-size: 13px;
  color: #777;
  margin-top: 4px;
}

.webgis-title {
  font-family: 'SimHei', 'LiSu', serif;
  font-size: 36px;
  letter-spacing: 1.5px;
  white-space: nowrap;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
  color: white;
  background: linear-gradient(to right, #ffffff, #e6ffe6);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* 响应式调整 */
@media (max-width: 992px) {
  .navbar {
    flex-wrap: wrap;
  }
  
  .navbar ul {
    margin: 10px 0;
    flex: 1 0 100%;
    justify-content: center;
  }
  
  .login-wrapper {
    margin: 10px 0 0 0;
    align-self: flex-end;
  }
  
  .description-box {
    top: 55%;
    width: 90%;
    padding: 15px;
  }
  
  .description-text {
    font-size: 18px;
  }
  
  .slider-dots {
    bottom: 20px;
  }
}

@media (max-width: 768px) {
  .navbar ul {
    flex-direction: column;
    align-items: center;
  }
  
  .navbar li {
    margin: 5px 0;
  }
  
  .webgis-title {
    font-size: 28px;
    margin-bottom: 10px;
  }
  
  .description-text {
    font-size: 16px;
  }
  
  .arrow {
    width: 40px;
    height: 40px;
    font-size: 24px;
    margin: 0 15px;
  }
  
  .slider-dots span {
    width: 10px;
    height: 10px;
    margin-right: 10px;
  }
  
  .description-box {
    top: 50%;
    padding: 10px;
  }
}
</style>