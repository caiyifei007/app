<template>
	<div class="login">
		<van-nav-bar left-arrow @click-left="onClickLeft" />
		<div class="login_content">
			<login-top></login-top>
			<login-form :meurl="meurl"></login-form>
			<div class="third">
				<thirdparty
					v-for="(item, i) of url"
					:key="i"
					:src="require('../assets/img/login_reg/' + item)"
				></thirdparty>
			</div>
			<p class="protocol">
				<span>登录注册表示同意</span
				><router-link to="forget">用户许可使用协议、隐私政策</router-link>
			</p>
		</div>
	</div>
</template>

<script>
import LoginTop from "../components/LoginTop";
import LoginForm from "../components/LoginForm";
import ThirdParty from "../components/ThirdParty";
import { mapActions } from "vuex";
export default {
	components: {
		"login-top": LoginTop,
		"login-form": LoginForm,
		thirdparty: ThirdParty
	},
	data() {
		return {
			url: [
				"umeng_socialize_wechat.png",
				"umeng_socialize_sina.png",
				"umeng_socialize_qq1.png"
			],
			meurl: ""
		};
	},
	methods: {
		onClickLeft() {
			console.log(this.meurl);
			this.meurl ? this.$router.push(this.meurl) : this.$router.go(-1);
		},
		...mapActions(["getlogin"]),
		transmit(data) {
			this.meurl = data;
		}
	},
	created() {
		this.bus.$on("transmit", this.transmit);
	},
	destroyed() {
		this.getlogin();
	}
};
</script>

<style scoped lang="scss">
.login {
	background-image: url(../assets/img/login_reg/su_background.png);
	background-size: 100% 100%;
}
.van-nav-bar {
	background-color: transparent;
	&:after {
		border: none;
	}
}
.van-nav-bar .van-icon {
	color: #000;
}
.login_content {
	padding: 20px 40px;
	height: 530px;
	display: flex;
	flex-direction: column;
}
.third {
	display: flex;
	justify-content: space-between;
}
.protocol {
	font-size: 10px;
	margin: 20px 10px;
}
a {
	color: rgba(217, 46, 49, 1);
}
</style>
