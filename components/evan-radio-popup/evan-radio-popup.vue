<template>
	<view class="evan-radio-popup">
		<view class="evan-radio-popup__trigger" @click="openPopup">
			<slot name="trigger" :label="label"></slot>
			<slot></slot>
		</view>
		<uni-popup @change="onPopupChange" ref="popup" type="bottom" :maskClick="maskClick">
			<view class="evan-radio-popup__target">
				<view class="evan-radio-popup__target__header">
					<text @click="onCancel" class="evan-radio-popup__target__header__cancel">{{cancelText}}</text>
					<text class="evan-radio-popup__target__header__title">{{title}}</text>
					<text @click="onConfirm" class="evan-radio-popup__target__header__confirm" :style="{color:primaryColor}">{{confirmText}}</text>
				</view>
				<scroll-view scroll-y="true" class="evan-radio-popup__target__body">
					<evan-radio-group v-model="currentValue">
						<view @click="onRadioClick(index)" class="evan-radio-popup__target__body__listitem" v-for="(item,index) in options"
						 :key="item[optionValue]">
							<text class="evan-radio-popup__target__body__listitem__label" :style="{color:currentValue===item[optionValue]?primaryColor:'#333'}">{{item[optionLabel]}}</text>
							<evan-radio :clearable="clearable" :primaryColor="primaryColor" ref="radio" :preventClick="true" :label="item[optionValue]">
								<template slot="icon">
									<view>
										<uni-icons v-if="currentValue===item[optionValue]" type="checkmarkempty" size="25" :color="primaryColor"></uni-icons>
									</view>
								</template>
							</evan-radio>
						</view>
					</evan-radio-group>
				</scroll-view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import EvanRadio from '@/components/evan-radio/evan-radio.vue'
	import EvanRadioGroup from '@/components/evan-radio-group/evan-radio-group.vue'
	import uniPopup from '@/components/uni-popup/uni-popup.vue'
	import uniIcons from '@/components/uni-icons/uni-icons.vue'
	export default {
		name: 'EvanRadioPopup',
		components: {
			EvanRadio,
			EvanRadioGroup,
			uniPopup,
			uniIcons
		},
		props: {
			options: {
				type: Array,
				default: () => []
			},
			value: {
				type: [String, Number, Boolean],
				default: null
			},
			primaryColor: {
				type: String,
				default: '#108ee9'
			},
			cancelText: {
				type: String,
				default: '取消'
			},
			confirmText: {
				type: String,
				default: '确定'
			},
			title: {
				type: String,
				default: '请选择'
			},
			optionLabel: {
				type: String,
				default: 'label'
			},
			optionValue: {
				type: String,
				default: 'value'
			},
			maskClick: {
				type: Boolean,
				default: true
			},
			clearable: {
				type: Boolean,
				default: false
			}
		},
		computed: {
			label() {
				if (this.isTrueEmpty(this.value)) {
					return ''
				}
				const selectedOption = this.options.find((op) => op[this.optionValue] === this.value)
				if (selectedOption) {
					return selectedOption[this.optionLabel]
				}
				return ''
			}
		},
		data() {
			return {
				currentValue: null,
				maskClose: true // 是否由点击遮罩层关闭
			}
		},
		methods: {
			openPopup() {
				this.currentValue = this.value
				this.$refs.popup.open()
			},
			closePopup() {
				this.maskClose = false
				this.$refs.popup.close()
			},
			onCancel() {
				this.closePopup()
				this.$emit('cancel', this.currentValue)
			},
			onConfirm() {
				this.closePopup()
				let obj = null
				if (!this.isTrueEmpty(this.currentValue)) {
					obj = this.options.find((op) => op[this.optionValue] === this.currentValue)
				}
				this.$emit('input', this.currentValue)
				this.$emit('confirm', this.currentValue)
				this.$emit('objConfirm', obj)
			},
			onRadioClick(index) {
				this.$refs.radio[index].select()
			},
			onPopupChange(e) {
				// 捕捉uniPopup点击遮罩层关闭事件
				if (!e.show) {
					if (this.maskClose) {
						this.$emit('cancel', this.currentValue)
					}
					this.maskClose = true
				}
			},
			isTrueEmpty(str) {
				if (str || str === 0 || str === false) {
					return false
				}
				return true
			}
		}
	}
</script>

<style lang="scss">
	.evan-radio-popup {}

	.evan-radio-popup__target {}

	.evan-radio-popup__target__header {
		height: 54px;
		background-color: #f7f7f7;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		align-items: center;
		font-size: 16px;
	}

	.evan-radio-popup__target__header__cancel {
		color: #999;
		padding: 0 15px;
	}

	.evan-radio-popup__target__header__title {
		color: #333;
		flex: 1;
		text-align: center;
	}

	.evan-radio-popup__target__header__confirm {
		padding: 0 15px;
	}

	.evan-radio-popup__target__body {
		background-color: #fff;
		/* #ifndef APP-NVUE */
		max-height: 350px;
		/* #endif */
		/* #ifdef APP-NVUE */
		maxHeight: 350px;
		/* #endif */
	}

	.evan-radio-popup__target__body__listitem {
		align-items: center;
		height: 50px;
		padding: 0 15px;
		border-bottom-width: 1px;
		border-bottom-style: solid;
		border-bottom-color: #eee;
		/* #ifndef APP-NVUE */
		display: flex;
		box-sizing: border-box;
		/* #endif */
		flex-direction: row;
	}

	.evan-radio-popup__target__body__listitem__label {
		font-size: 16px;
		color: #333;
		flex: 1;
		margin-right: 6px;
	}
</style>
