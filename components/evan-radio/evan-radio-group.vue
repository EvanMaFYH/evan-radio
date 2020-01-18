<template>
	<view class="evan-radio-group">
		<slot></slot>
	</view>
</template>

<script>
	export default {
		name: 'EvanRadioGroup',
		props: {
			value: {
				type: [String, Number, Boolean],
				default: null
			},
			disabled: {
				type: Boolean,
				default: false
			}
		},
		watch: {
			value: {
				handler(value) {
					this.deepSetValue(this.$children)
				}
			}
		},
		methods: {
			onRadioChange(label) {
				this.$emit('input', label)
				this.$emit('change', label)
			},
			deepSetValue(array) {
				if (Array.isArray(array)) {
					array.forEach((child) => {
						let childName = child.$options.name
						if (childName === 'EvanRadio') {
							if (typeof child.setValue === 'function') {
								child.setValue(this.value)
							}
						} else if (child.$children) {
							this.deepSetValue(child.$children)
						}
					})
				}
			}
		}
	}
</script>

<style>
</style>
