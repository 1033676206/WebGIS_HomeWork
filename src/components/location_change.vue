<template>
  <div>
      <nav class="navbar">
    <!-- 导航栏 -->
    <div class="logo">
          <h1 class="webgis-title" >华航WebGIS</h1>
        </div>
        <ul class="nav-links">
            <li>
              <div class="location-option">
                    <router-link to='/feedback' >学校历史</router-link>
              </div>
          </li>
            <li>  <router-link to='/complex' >地理概览</router-link></li>
            <li><router-link to='/history'>校史文化标注</router-link></li>
            <li><router-link to='/schoolmate'>知名校友</router-link></li>
            <li> <router-link to='/feedback' >用户反馈</router-link></li>                         
          </ul>
        <div class="login-wrapper">
          <button class="login-button" @mouseenter="showOptions = true" @mouseleave="hideOptions">
<span>登录</span>
<span class="circle"></span>
<span class="arrow-icon" :class="{ 'up': showOptions }"></span>
</button>

<ul class="user-options" :style="{ display: showOptions ? 'block' : 'none' }"
@mouseenter="showOptions = true" @mouseleave="showOptions = false">
<li @click="redirectToPage('/normaluser')"   >普通用户</li>
<li @click="redirectToPage('/superuser')"  @mouseleave="showOptions = false" >管理员</li>
</ul>
</div>
    </nav>
    <div class="parent">
<div class="timeline">
  
<section id="timeline" class="timeline-outer">
   <h3 class="header">历史沿革</h3 >
  <div class="container" id="content">
        <div class="row">
      <div class="col s12 m12 l12">
        <h2 class="blue-text lighten-1 header"></h2>
        <ul class="timeline">
          <li v-for="time in timelineItems" :key="time" @click="selectTime(time)">
  {{ time }}
</li>
          <li class="event" data-date="1978-1985">
            <h3>创立初期</h3>
            <p>
              1978年，北华航天工业学院的前身——华北航天工业学院在河北省廊坊市正式创立，由原第五机械工业部投资建设，旨在为航天、兵器等国防工业培养专业技术人才。1980年，学院开始面向全国招生，最初设有机械制造、电子工程、计算机科学等专业，招生规模较小，但为后续发展奠定了基础。到了1985年，随着国防工业的调整和发展，学院逐步扩大招生规模，增设了多个与航天、航空相关的专业，如航天工程、飞行器制造等，同时不断完善教学设施。</p>
          </li>
          <li class="event" data-date="1986-1995">
            <h3>稳步发展</h3>
            <p>
              1986年，华北航天工业学院正式成为硕士学位授予单位，开始培养硕士研究生，这标志着学院在科研和教学水平上的显著提升。进入1990年，学院与多家航天企业建立了紧密的合作关系，积极开展产学研合作项目，这不仅为学生提供了宝贵的实习和就业机会，也促进了学院科研成果的转化。到1995年，学院顺利通过了国家教育部门的本科教学合格评估，教学质量得到了官方认可，学院的社会声誉也随之进一步提升。</p>

          </li>
          <li class="event" data-date="1996-2005">
            <h3>转型与扩建</h3>
            <p>1996年，学院正式更名为华北航天工业学院，这一更名代表着学院在教学、科研等方面进入了新的发展阶段。进入2000年，学院开始大规模建设新的校区，以此来扩大办学规模，同时积极引进国内外优秀人才，师资队伍不断壮大。到了2005年，学院新增了多个本科专业，涵盖了工学、管理学、文学等多个学科领域，逐步发展成为一所多学科协调发展的高等学府。</p>
          </li>
          <li class="event" data-date="2006-2015">
            <h3>提升与突破</h3>
            <p>2006年，华北航天工业学院被确定为河北省重点建设的骨干大学之一，从而获得了更多的政策和资金支持。到了2010极的科研氛围，科研成果显著增加，在航天、航空、兵器等领域承担了多项国家级科研项目，科研实力不断增强。2015年，学院顺利通过了教育部本科教学工作审核评估，这标志着学院在教学质量和科研水平上又迈上了一个新的台阶。</p>
          </li>
          <li class="event" data-date="2016-至今">
            <h3>高质量发展</h3>
            <p>2016年，华北航天工业学院正式更名为北华航天工业学院，开启了新的发展征程。2018年，学院入选河北省"双一流"建设高校，重点发展航天、航空、兵器等优势学科。如今，北华航天工业学院在教学、科研、社会服务等方面都取得了显著成效，为国家的航天、航空和国防事业培养了大量高素质人才，成为一所具有鲜明航天特色、涵盖多学科的高等学府。</p>
          </li>
        </ul>
      </div>
    </div>
  </div>
</section>
</div>
<br/> 
<div class="map" id="map">
    <!-- 在这里插入地图组件 -->
  </div>
  <button @click="toggleFlightLine">迁移</button>
</div> 
  <!-- 底部版权信息 -->
  <footer class="footer">
        <p>copyright@2025
            <a href="https://www.nnu.edu.cn/">北华航天工业学院</a>
            版权所有</p>
            <h> 地址：河北省廊坊市爱民东道133号 邮箱：htzhshb@126.com</h>         
    </footer>
  </div>
</template>

<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';

export default {
  name: 'LocationPage',
  data() {
    return {
      timelineItems: [],
      selectedTime: null,
      showFlightLine: false,
      showOptions: false,
      map: null,
      markers: [],
      flightPath: null,
      locations: [
        {
          name: "北华航天工业学院(广阳校区)",
          time: "1978-至今",
          coordinates: [39.528887, 116.717813]
        },
        {
          name: "北华航天工业学院(固安校区)",
          time: "2022-至今",
          coordinates: [39.433106, 116.312317]
        }
      ],
      icon: null
    };
  },
  methods: {
    initTimeline() {
      const timelineItems = new Set();
      this.locations.forEach(location => {
        timelineItems.add(location.time);
      });
      this.timelineItems = Array.from(timelineItems).sort();
    },
    redirectToPage(path) {
      this.showOptions = false;
      this.$router.push(path);
    },
    selectTime(time) {
      this.selectedTime = time;
      this.showOptions = false;
      this.updateMarkers();
    },
    toggleFlightLine() {
      if (this.flightPath) {
        if (this.map.hasLayer(this.flightPath)) {
          this.map.removeLayer(this.flightPath);
        } else {
          this.map.addLayer(this.flightPath);
        }
      } else {
        this.addFlightPath();
      }
    },
    initMap() {
      // 正确初始化地图实例
      this.map = L.map('map').setView([39.52, 116.72], 12);

      // 添加底图
      const tiandituLayerImg = L.tileLayer(
        'https://t{s}.tianditu.gov.cn/DataServer?T=img_w&x={x}&y={y}&l={z}&tk={key}',
        {
          subdomains: ['0', '1', '2', '3', '4', '5', '6', '7'],
          key: '1550db5db74061f21422c8e6c265f371',
        }
      ).addTo(this.map);

      const tiandituLayerVec = L.tileLayer(
        'https://t{s}.tianditu.gov.cn/DataServer?T=vec_w&x={x}&y={y}&l={z}&tk={key}',
        {
          subdomains: ['0', '1', '2', '3', '4', '5', '6', '7'],
          key: '155极db5db74061f21422c8e6c265f371',
        }
      );

      // 图层控制
      const baseMaps = {
        '矢量图': tiandituLayerVec,
        '影像图': tiandituLayerImg,
      };
      
      L.control.layers(baseMaps).addTo(this.map);
      
      // 创建图标
      this.icon = L.icon({
        iconUrl: 'https://s1.ax1x.com/2023/06/15/pCK15y8.png',
        iconSize: [32, 32],
        iconAnchor: [16, 32],
        popupAnchor: [0, -32]
      });

      // 添加标记
      this.addMarkers();
    },
    addMarkers() {
      // 移除现有标记
      this.markers.forEach(marker => this.map.removeLayer(marker));
      this.markers = [];
      
      // 添加新标记
      this.locations.forEach(location => {
        const marker = L.marker(location.coordinates, { icon: this.icon })
          .addTo(this.map)
          .bindPopup(`<b>${location.name}</b><br>${location.time}<br>${location.coordinates.join(', ')}`);
        
        this.markers.push(marker);
      });
    },
    addFlightPath() {
      // 添加飞行路径
      this.flightPath = L.polyline(
        this.locations.map(loc => loc.coordinates),
        {
          color: '#ff7800',
          weight: 5,
          opacity: 0.7,
          dashArray: '10, 5'
        }
      ).addTo(this.map);
    },
    hideOptions() {
      this.showOptions = false;
    },
    updateMarkers() {
      if (!this.selectedTime) return;
      
      // 移除现有标记
      this.markers.forEach(marker => this.map.removeLayer(marker));
      this.markers = [];
      
      // 根据选择的时间添加标记
      const filteredLocations = this.locations.filter(loc => loc.time === this.selectedTime);
      filteredLocations.forEach(location => {
        const marker = L.marker(location.coordinates, { icon: this.icon })
          .addTo(this.map)
          .bindPopup(`<b>${location.name}</b><br>${location.time}<br>${location.coordinates.join(', ')}`);
        
        this.markers.push(marker);
      });
    }
  },
  mounted() {
    // 初始化所有组件
    this.initTimeline();
    this.initMap();
  }
};
</script>


<style>
/* 全局样式 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
 
}
.webgis-title {
font-family: "LiSu", "SimSun", serif;
font-size:50px;
}

/* 导航栏样式 */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 30px 20px;
  background-color: #227700;
  color: #ffffff;
  margin-top:-60px;
}
.n极bar ul {
  display: flex;
  list-style: none;
}
.navbar li {
  margin-right: 20px;
}
.navbar li a {
  color: #ffffff;
  text-decoration: none;
}

.location-option {
background-color: rgba(255, 255, 255, 0.5);
border-radius: 4px;
padding: 6px;
margin-top:-5px
}

.location-option:hover {
background-color: rgba(255, 255, 255, 0.7);
}


.login-wrapper {
position: relative;
}

.login-button {
position: relative;
display: flex;
justify-content: center;
align-items: center;
padding: 16px;
background-color: #fff;
box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
border-radius: 24px;
border: none;
cursor: pointer;
font-size: 14px;
color: #333;
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
}

.arrow-icon {
position: absolute;
top: calc(50% - 3px);
right: 16px;
width: 0;
height: 0;
border: solid #999;
border-width: 0 2px 2px 0;
display: inline-block;
padding: 3px;
transition: transform 0.3s ease-in-out;
}

.arrow-icon.up {
transform: rotate(-135deg);
}

.user-options {
position: absolute;
top: calc(100% + 8px);
right: 0;
width: 120px;
background-color: #fff;
box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
border-radius: 4px;
border: none;
list-style: none;
padding: 8px;
}

.user-options li {
cursor: pointer;
text-align: center;
margin: 8px 0;
color:#000000
}

.user-options li:hover {
background-color: #f5f5f5;
}

body {
font-family: helvetica;
}
/*——————————————
TimeLine CSS
———————————————*/
/* Base */

#content {
margin-top: 50px;
text-align: center;

}

section.timeline-outer {
width: 200%;
margin: 0 auto;
margin-left:110px;
}

h1.header {
font-size: 50px;
line-height: 70px;

}
.header {
font-size: 50px;
line-height: 70px;
margin-left:80px;
margin-bottom: -100px; /* 调整标题位置的值 */
}
/* Timeline */

.timeline {
float:left;
width: 40%;

border-left: 8px solid #42A5F5;
border-bottom-right-radius: 2px;
border-top-right-radius: 2px;
box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.16), 0 2px 10px 0 rgba(0, 0, 0, 0.12);
color: #333;
margin: 50px auto;
letter-spacing: 0.5px;
position: relative;
line-height: 1.4em;
padding: 20px;
list-style: none;
text-align: left;

}
#map {
width: 60%;
height: 1050px;
margin: 50px;
border: 1px solid #ccc;
float:right;
/* 其他样式属性 */
}

.parent {
display: flex;
flex-wrap: wrap;
justify-content: space-between;
}
.timeline h1,
.timeline h2,
.timeline h3 {
font-size: 1.4em;
}

.timeline .event {
border-bottom: 1px solid rgba(160, 160, 160, 0.2);
padding-bottom: 15px;
margin-bottom: 20px;
position: relative;
}

.timeline .event:last-of-type {
padding-bottom: 0;
margin-bottom: 0;
border: none;
}

.timeline .event:before,
.timeline .event:after {
position: absolute;
display: block;
top: 0;
}

.timeline .event:before {
left: -177.5px;
color: #212121;
content: attr(data-date);
text-align: right;
/*  font-weight: 100;*/

font-size: 16px;
min-width: 120px;
}

.timeline .event:after {
box-shadow: 0 0 0 8px #42A5F5;
left: -30px;
background: #212121;
border-radius: 50%;
height: 11px;
width: 11px;
content: "";
top: 5px;
}
/**/
/*——————————————
Responsive Stuff
———————————————*/

@media (max-width: 945px) {
.timeline .event::before {
  left: 0.5px;
  top: 20px;
  min-width: 0;
  font-size: 13px;
}
.timeline h3 {
  font-size: 16px;
}
.timeline p {
  padding-top: 20px;
}
section.lab h3.card-title {
  padding: 5px;
  font-size: 16px
}
}

@media (max-width: 768px) {
.timeline .event::before {
  left: 0.5px;
  top: 20px;
  min-width: 0;
  font-size: 13px;
}
.timeline .event:nth-child(1)::before,
.timeline .event:nth-child(3)::before,
.timeline .event:nth-child(5)::before {
  top: 38px;
}
.timeline h3 {
  font-size: 16px;
}
.timeline p {
  padding-top: 20px;
}
}
/*——————————————
others
———————————————*/

a.portfolio-link {
margin: 0 auto;
display: block;
text-align: center;
}
.map {
flex: 1;
/* 在这里设置地图的样式 */
}
/* 底部样式 */
.footer {
background-color: #f2f2f2;
;
padding: 1rem;
text-align: center;
z-index: 999; /* 设置一个较高的 z-index 值 */
}

.footer a {
color: #000000;
text-decoration: none;
}

.footer p {
margin: 0;
padding: 0;
color: #666666;
}
</style>