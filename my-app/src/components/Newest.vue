<template>
	<div class="soical">
		<div v-for="(item, i) in posts" :key="i">
			<van-row type="flex" justify="space-between">
				<van-col span="16">
					<van-row type="flex">
						<van-col span="6">
							<img :src="serverBaseURL + item.user_img" alt="" class="tx" />
						</van-col>
						<van-col span="18">
							<div class="nichen">{{ item.uname }}<em class="xz"></em></div>
							<div class="gxqm">{{ item.say }}</div>
						</van-col>
					</van-row>
				</van-col>
				<van-col span="8">
					<button type="primary" @click="alertMenu" class="fx">···</button>
					<button class="gz" @click="guanzhu">+关注</button>
					<van-action-sheet
						v-model="show"
						:actions="actions"
						cancel-text="取消"
						@select="onSelect"
					/>
				</van-col>
			</van-row>
			<p class="say">
				<a href="#">#{{ item.label }}#</a>{{ item.content }}
			</p>
			<div class="tp">
				<van-image
					width="6rem"
					height="6rem"
					fit="cover"
					v-for="(img, index) of JSON.parse(item.images)"
					:key="index"
					:src="serverBaseURL + img"
				/>
			</div>
			<van-row type="flex" justify="space-between" class="buttoms">
				<van-col span="16">
					<div>
						<van-col span="8.5">
							<p class="publish">
								发布于 星期{{ changeChinese(item.post_time) }}
							</p>
						</van-col>
					</div>
				</van-col>
				<van-col span="0" class="dzpl">
					<a href="#"></a> <span>{{ item.comments }}</span>
					<em @click="like"></em><span>{{ item.likes }}</span>
				</van-col>
			</van-row>
			<hr />
		</div>
	</div>
</template>
<script>
import { mapState } from "vuex";

export default {
	data() {
		return {
			show: false,
			actions: [{ name: "收藏" }, { name: "举报" }],
			posts: [],
			userSay: [
				{
					headPhoto: "assets/img/community/tx1.jpg",
					nickname: "Hi_小芋头",
					hydj: "1",
					gxqm: "番茄",
					jhss: "寻找同城捞友",
					ss: "有木有长春的小伙伴啊?不想一个人吃啊！",
					msimg: [
						"assets/img/community/ms1.jpg",
						"assets/img/community/ms2.jpg",
						"assets/img/community/ms3.jpg",
						"assets/img/community/ms4.jpg",
						"assets/img/community/ms5.jpg",
						"assets/img/community/ms6.jpg",
						"assets/img/community/ms7.jpg"
					],
					fby: "星期天",
					from: "海捞天地",
					plgs: "5",
					dzgs: "30"
				}
			]
		};
	},
	methods: {
		changeChinese(time) {
			switch (new Date(time).getDay()) {
				case 0:
					return "日";
					break;
				case 1:
					return "一";
					break;
				case 2:
					return "二";
					break;
				case 3:
					return "三";
					break;
				case 4:
					return "四";
					break;
				case 5:
					return "五";
					break;
				case 6:
					return "六";
			}
		},
		alertMenu() {
			this.show = true;
		},
		onSelect(item) {
			// 默认情况下，点击选项时不会自动关闭菜单
			// 可以通过 close-on-click-action 属性开启自动关闭
			this.show = false;
			this.$toast(item.name);
		},
		guanzhu(e) {
			// var gzbuts = document.getElementsByClassName("gz");
			// gzbuts.style.display = "none";
			// console.log(gzbuts);
			e.currentTarget.style.display = "none";

			this.$toast("关注成功");
		},
		like(e) {
			var num = parseInt(e.target.nextSibling.innerHTML);
			// e.target.style.remove("active");
			if (e.target.getAttribute("class") !== "active") {
				e.target.nextSibling.innerHTML = num + 1;
				e.target.classList.add("active");
			} else {
				e.target.classList.remove("active");
				e.target.nextSibling.innerHTML = num - 1;
			}
		}
	},
	computed: {
		...mapState(["serverBaseURL"])
	},
	created() {
		this.$api.getNewPosts().then(res => {
			console.log(res);
			this.posts = res.data.result;
		});
	}
};
</script>
<style scoped>
.soical {
	margin-top: 10px;
	padding: 10px;
}
.nichen {
	text-align: left;
	font-size: 13px;
}
.gxqm {
	margin-top: 8px;
	text-align: left;
	font-size: 10px;
	color: #9a9a9a;
}
.xz {
	display: inline-block;
	width: 13px;
	height: 14.5px;
	background: url(../assets/img/community/search_golden_icon.png);
	background-size: 100% 100%;
	vertical-align: middle;
	margin-left: 5px;
}
.gz {
	float: right;
	background: #fff;
	border-radius: 50px;
	border: 1px solid #c97d7f;
	font-size: 10px;
	color: #e70012;
	width: 51px;
	height: 24px;
	box-sizing: border-box;
}
.fx {
	float: right;
	font-size: 10px;
	color: #e70012;
	font-weight: 1000;
	border: none;
	letter-spacing: 3px;
	height: 24px;
	background: transparent;
}
.tx {
	width: 40px;
	border-radius: 50%;
	margin-right: 10px;
}
.say {
	margin: 10px 0;
	font-size: 13px;
	text-align: left;
	line-height: 17px;
}
a {
	font-size: 13px;
	color: #e70012;
}
.tp {
	width: 100%;
	text-align: left;
}
.tp > div {
	margin-right: 4px;
}
.publish {
	display: inline;
	height: 16px;
	font-size: 10px;
	color: #9a9a9a;
	line-height: 16px;
}

.dzpl {
}
.dzpl > a {
	display: inline-block;
	width: 16px;
	height: 16px;
	background: url(../assets/img/community/icon_10.png);
	background-size: 100% 100%;
	vertical-align: middle;
}
.dzpl > em {
	cursor: pointer;
	margin-left: 10px;
	display: inline-block;
	width: 16px;
	height: 16px;
	background-image: url(../assets/img/community/icon_13.png);
	background-size: 100% 100%;
	vertical-align: middle;
}
.dzpl > em.active {
	background-image: url(../assets/img/community/icon_21.png);
}
.dzpl > span {
	font-size: 10px;
	color: #9a9a9a;
}
hr {
	border: 1px solid #f1f1f1;
}
.buttoms {
	margin-top: 10px;
}
</style>
