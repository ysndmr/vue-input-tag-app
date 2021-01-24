<template>
	<div class="input__group">
		<small class="input__group--info">* You can add up to {{ maxTag }} tags </small>
		<div class="input__group--wrapper">
				<span class="input__group--span" v-for="(tag, index) in tags" v-bind:key="tag.key">
					<span class="input__group--value">
						{{ tag }}
					</span>
					<span class="input__group--close" @click="removeTag($event, index)"> X </span>
				</span>
			<input class="input__group--input"
						 @keydown.enter="valueCheck"
						 @keydown.tab.stop.prevent="valueCheck"
						 @keydown.backspace="removeTag($event, undefined)"
						 type="text"
						 placeholder="write tags, least 4 letters.">
		
		</div>
		<transition name="fade">
			<span class="input__group--error" v-if="isVisible">This tag already added</span>
			<span class="input__group--error" v-if="isError">You added already seven tag</span>
		</transition>
	</div>
</template>

<script>
export default {
	name: 'InputTags',
	data() {
		return {
			tags: [],
			isVisible: false,
			isError: false,
			maxTag: 7,
			minLetters: 4
		}
	},
	methods: {
		valueCheck(event) {
			let evenTarget = event.target;
			let spanCheck = document.getElementsByClassName("input__group--span");
			let validate = false;
			if (evenTarget.value.length >= this.minLetters) {
				this.tags.forEach(tagValue => {
					if (tagValue.toLowerCase() === evenTarget.value.toLowerCase()) {
						validate = true;
					}
				})
				if ( !validate) {
					if (spanCheck.length < this.maxTag) {
						this.isError = false
						this.addTags(evenTarget)
					} else {
						this.isError = true;
						setTimeout(() => {
							this.isError = false
						}, 3000)
					}
				} else {
					this.noAddTags();
				}
				evenTarget.value = ''
			}
		},
		addTags(event) {
			this.tags.push(event.value);
			this.isVisible = false
		},
		noAddTags() {
			this.isVisible = true
			setTimeout(() => {
				this.isVisible = false
			}, 3000)
		},
		removeTag(event, index) {
			if (index == undefined) {
				if (event.target.value <= 0) {
					console.log(event.target.value)
					this.tags.splice(this.tags.length - 1, 1)
				}
			} else {
				this.tags.splice(index, 1)
			}
			
			
		}
	}
}
</script>

<style lang="scss">
.input__group {
	text-align: center;
	padding: 0 20px;
	width: 100%;
	max-width: 1140px;
	margin: 0 auto;
	
	&--wrapper {
		border: 1px solid #ccc;
		width: 100%;
		height: 50px;
		margin: 15px 0;
		display: flex;
		align-items: center;
		padding: 5px 10px;
		border-radius: 5px;
	}
	
	&--input {
		margin: 0;
		height: 100%;
		padding: 5px 10px;
		border: 0;
		outline: none;
	}
	
	&--span {
		background-color: #41b883;
		padding: 5px 10px;
		color: white;
		cursor: default;
		border-radius: 5px;
		margin-right: 7px;
		font-weight: bold;
		text-shadow: 1px 1px 1px #000000;
	}
	
	&--close {
		cursor: pointer;
		font-size: 12px
	}
	
	&--error {
		color: darkred;
	}
	
	&--info {
		display: flex;
		font-size: 11px;
		color: #2d3e4f
	}
}

.fade-enter-active,
.fade-leave-active {
	transition: opacity .5s
}

.fade-enter,
.fade-leave-to {
	opacity: 0
}
</style>
