<template>
  <div id="app" :style="{ height: `${innerHei}px` }" @touchmove="touchmove" @touchend="touchend">
		<kun-sidebar :list="sliderBar" :showPage="showPage" @jumpPage="jumpPage"></kun-sidebar>
		<div class="content" :style="{ height: `${innerHei}px`, transform: `translateY(${nowTop}px)` }">
			<my-index :info="slider.index"></my-index>
			<my-intro :info="slider.intro"></my-intro>
			<my-skills :info="slider.skills"></my-skills>
			<my-project :info="slider.project"></my-project>
		</div>
  </div>
</template>

<script>
import myIndex from '@/pages/myIndex';
import myIntro from '@/pages/myIntro';
import mySkills from '@/pages/mySkills';
import myProject from '@/pages/myProject';
import kunSidebar from '@/components/kunSidebar';
export default {
	data() {
		return {
			innerHei: 0,
			showPage: 0,
			isSliding: false,
			slider: {
				index: {
					name: '林柏坤',
					intro: '大三学生, 前端初学探索中...',
					links: [
						{
							label: 'Blog',
							icon: '#icon-blogease',
							href: 'https://kunine.github.io/blog'
						},
						{
							label: 'GitHub',
							icon: '#icon-github',
							href: 'https://www.github.com/Kunine	'
						},
						{
							label: 'Coding',
							icon: '#icon-icon',
							href: 'https://coding.net/u/Kunine'
						}
					]
				},
				intro: {
					title: '个人介绍',
					intro:
            '大一开始接触前端开发，结合学校课程，向着前端路线不断探索中，学习接触过JavaScript、NodeJs、PHP、Java...，目前正在准备专升本复习中。',
					icons: [
						{
							icon: '#icon-birthday',
							label: '1997-08'
						},
						{
							icon: '#icon-edu',
							label: '广东轻工职业技术学院'
						},
						{
							icon: '#icon-position',
							label: '广东·广州'
						},
						{
							icon: '#icon-youxiang',
							label: 'ninebook@hotmail.com'
						},
						{
							icon: '#icon-phone',
							label: '13631539770'
						},
						{
							icon: '#icon-CombinedShape',
							label: 'LBK865996'
						}
					]
				},
				skills: [
					{
						path: '/static/img/html.png',
						label: 'HTML'
					},
					{
						path: '/static/img/css.png',
						label: 'CSS'
					},
					{
						path: '/static/img/javascript.png',
						label: 'JavaScript'
					},
					{
						path: '/static/img/nodejs.png',
						label: 'Node.Js'
					},
					{
						path: '/static/img/php.png',
						label: 'PHP'
					},
					{
						path: '/static/img/maya.png',
						label: 'MAYA'
					},
					{
						path: '/static/img/photoshop.png',
						label: 'PhotoShop'
					},
					{
						path: '/static/img/ai.png',
						label: 'illustrator'
					}
				],
				project: {
					github: 'https://github.com/Kunine/',
					project: [
						{
							label: 'mallBackend',
							url: '/static/img/mallBackend.jpg',
							intro: '使用Vue & elmentUI 开发的一个商城类后台管理模板，包含用户商品管理，发布商品以及商城概况。'
						},
						{
							label: 'RedPacket',
							url: '/static/img/RedPacket.jpg',
							intro:
                '使用 VueJs 开发的微信端手机端红包分享Web应用。'
						},
						{
							label: 'Vue-HomeEconomicsWebsides',
							url: '/static/img/Vue-HomeEconomicsWebsides.jpg',
							intro: '使用 HTML/CSS/JavaScript 开发的自使用Web官网。'
						}
					]
				},
				blog: {}
			},
			sliderBar: [
				{
					label: '个人简介',
					icon: '#icon-index'
				},
				{
					label: '个人介绍',
					icon: '#icon-user'
				},
				{
					label: '专业技能',
					icon: '#icon-jineng-copy'
				},
				{
					label: '项目经历',
					icon: '#icon-jingli'
				}
			],
			nowTop: 0
		};
	},
	components: {
		myIndex,
		myIntro,
		mySkills,
		myProject,
		kunSidebar
	},
	watch: {
		showPage() {
			this.delay();
			this.nowTop = parseInt(this.showPage) * parseInt(this.innerHei) * -1;
		}
	},
	methods: {
		touchmove(e) {
			e.preventDefault();
			if (this.touchStartX !== 0) return;
			this.touchStartX = e.changedTouches[0].screenY;
		},
		touchend(e) {
			e.preventDefault();

			if (this.touchStartX === 0) return;

			const touchEndX = e.changedTouches[0].screenY;

			if (this.isSliding) return;

			if (this.touchStartX - touchEndX > 60) {
				this.isSliding = true;
				if (this.showPage < Object.keys(this.slider).length - 2) { this.showPage++; }
			} else if (this.touchStartX - touchEndX < -60) {
				this.isSliding = true;
				if (this.showPage > 0) this.showPage--;
			}

			this.touchStartX = 0;
		},
		jumpPage(page) {
			if (this.isSliding) return;
			this.showPage = page;
		},
		delay() {
			this.isSliding = true;
			setTimeout(() => {
				this.isSliding = false;
			}, 1000);
		}
	},
	mounted() {
		this.innerHei = window.innerHeight;

		window.onmousewheel = e => {
			if (this.isSliding) return;
			this.delay();
			e.stopPropagation();
			e.preventDefault();

			//* 向下滚动
			if (e.wheelDelta < 40) {
				this.isSliding = true;
				if (this.showPage < Object.keys(this.slider).length - 2) { this.showPage++; }
			}
			if (e.wheelDelta > 40) {
				this.isSliding = true;
				if (this.showPage > 0) this.showPage--;
			}
		};
	}
};
</script>

<style lang="scss">
@import "./assets/css/base";
#app {
  min-width: 320px;
  overflow: hidden;
  position: relative;
  h1 {
    padding: 60px 0;
		transition: all .5s ease-in;
    color: #eee;
    text-align: center;
    text-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
		@media screen and (max-width: 1200px){
			padding: 50px 0;
		}
		@media screen and (max-width: 10254px){
			padding: 40px 0;
		}
		@media screen and (max-width: 800px){
			padding: 30px 0;
		}
		@media screen and (max-width: 374px){
			padding: 20px 0;
		}
  }
  .content {
    position: absolute;
    top: 0;
    transition: all .5s ease-out;
    width: 100%;
    left: 0;
    & > div {
      overflow: hidden;
      height: 100%;
    }
  }
}

@media screen and (max-width: 800px) {
  #kun-sidebar {
    display: none;
  }
}
</style>
