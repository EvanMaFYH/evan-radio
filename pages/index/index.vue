<template>
	<view class="evan-radio-show">
		<view class="evan-radio-show__title">
			<text class="evan-radio-show__title__item">基础用法</text>
		</view>
		<evan-radio v-model="baseValue" label="base1">基础用法1</evan-radio>
		<evan-radio v-model="baseValue" label="base2">基础用法2</evan-radio>

		<view class="evan-radio-show__title">
			<text class="evan-radio-show__title__item">自定义颜色</text>
		</view>
		<evan-radio v-model="primaryColorValue" primaryColor="#00a231" label="primaryColor1">自定义颜色1</evan-radio>
		<evan-radio v-model="primaryColorValue" primaryColor="#00a231" label="primaryColor2">自定义颜色2</evan-radio>

		<view class="evan-radio-show__title">
			<text class="evan-radio-show__title__item">自定义形状</text>
		</view>
		<evan-radio v-model="shapeValue" shape="square" label="shape1">自定义形状1</evan-radio>
		<evan-radio v-model="shapeValue" shape="square" label="shape2">自定义形状2</evan-radio>

		<view class="evan-radio-show__title">
			<text class="evan-radio-show__title__item">自定义图标来自uni-icons</text>
		</view>
		<evan-radio v-model="uniValue" icon="locked" label="uni1">自定义图标来1</evan-radio>
		<evan-radio v-model="uniValue" icon="locked" label="uni2">自定义图标来2</evan-radio>

		<view class="evan-radio-show__title">
			<text class="evan-radio-show__title__item">完全自定义图标</text>
		</view>
		<evan-radio v-model="iconValue" label="icon1">
			<text>完全自定义图标1</text>
			<template slot="icon">
				<uni-icons type="chat" size="20" :color="iconValue==='icon1'?'#108ee9':'#c8c9cc'"></uni-icons>
			</template>
		</evan-radio>
		<evan-radio v-model="iconValue" label="icon2">
			<text>完全自定义图标2</text>
			<template slot="icon">
				<uni-icons type="chat" size="20" :color="iconValue==='icon2'?'#108ee9':'#c8c9cc'"></uni-icons>
			</template>
		</evan-radio>

		<view class="evan-radio-show__title">
			<text class="evan-radio-show__title__item">自定义大小</text>
		</view>
		<evan-radio v-model="sizeValue" :iconSize="30" :titleStyle="{'font-size':'22px',color:'red'}" label="size1">自定义大小1</evan-radio>
		<evan-radio v-model="sizeValue" :iconSize="30" :titleStyle="{'font-size':'22px',color:'red'}" label="size2">自定义大小2</evan-radio>

		<view class="evan-radio-show__title">
			<text class="evan-radio-show__title__item">禁用</text>
		</view>
		<evan-radio v-model="disabledValue" :disabled="true" label="disabled1">禁用1</evan-radio>
		<evan-radio v-model="disabledValue" :disabled="true" label="disabled2">禁用2</evan-radio>

		<view class="evan-radio-show__title">
			<text class="evan-radio-show__title__item">单选框组</text>
		</view>

		<evan-radio-group @change="onGroupChange" v-model="color1">
			<view class="evan-radio-show__group-item" v-for="item in colorList" :key="item.value">
				<evan-radio :label="item.value">{{item.label}}</evan-radio>
			</view>
		</evan-radio-group>

		<view class="evan-radio-show__title">
			<text class="evan-radio-show__title__item">选中项可以取消</text>
		</view>
		<evan-radio-group @change="onGroupChange" v-model="colorClear">
			<view class="evan-radio-show__group-item" v-for="item in colorList" :key="item.value">
				<evan-radio clearable :label="item.value">{{item.label}}</evan-radio>
			</view>
		</evan-radio-group>

		<view class="evan-radio-show__title">
			<text class="evan-radio-show__title__item">自定义样式列表单选框组</text>
		</view>
		<evan-radio-group @change="onGroupChange" v-model="color2">
			<view @click="onRadioClick(index)" class="evan-radio-show__list-item" v-for="(item,index) in colorList" :key="item.value">
				<text class="evan-radio-show__list-item__label">{{item.label}}</text>
				<evan-radio ref="listRadio" :preventClick="true" :label="item.value">
					<template slot="icon">
						<view>
							<uni-icons v-if="color2===item.value" type="checkmarkempty" size="26" color="#108ee9"></uni-icons>
						</view>
					</template>
				</evan-radio>
			</view>
		</evan-radio-group>

		<view class="evan-radio-show__title">
			<text class="evan-radio-show__title__item">popup模式基本用法</text>
		</view>

		<evan-radio-popup v-model="color2" @confirm="onGroupChange" @objConfirm="onObjConfirm" :options="colorList">
			<template v-slot:trigger="{label}">
				<view>{{label||'请选择'}}</view>
			</template>
		</evan-radio-popup>
		<!-- #ifdef APP-PLUS -->
		<button @click="goNvue">nvue页面</button>
		<!-- #endif -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				radioValue: 'base',
				baseValue: 'base1',
				primaryColorValue: 'primaryColor1',
				shapeValue: 'shape1',
				uniValue: 'uni1',
				iconValue: 'icon1',
				sizeValue: 'size1',
				disabledValue: 'disabled1',
				color1: 'red',
				color2: 'green',
				colorClear: 'red',
				colorList: [{
						label: '红色',
						value: 'red'
					},
					{
						label: '绿色',
						value: 'green'
					},
					{
						label: '蓝色',
						value: 'blue'
					},
					{
						label: '粉色',
						value: 'pink'
					},
					{
						label: '黑色',
						value: 'black'
					},
				]
			}
		},
		onLoad() {

		},
		methods: {
			onGroupChange(e) {
				console.log(e)
			},
			onObjConfirm(e) {
				console.log(e)
			},
			onRadioClick(index) {
				this.$refs.listRadio[index].select()
			},
			goNvue() {
				uni.navigateTo({
					url: '/pages/app/app'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.evan-radio-show {
		padding: 20px;

		&__title {
			padding: 10px 0;

			&__item {
				font-size: 16px;
				color: #333;
				font-weight: bold;
			}
		}

		&__group-item {
			margin-bottom: 10px;
		}

		&__list-item {
			display: flex;
			align-items: center;
			height: 50px;
			border-bottom: 1px solid #eee;

			&__label {
				font-size: 16px;
				color: #333;
				flex: 1;
				margin-right: 6px;
			}
		}
	}
</style>
