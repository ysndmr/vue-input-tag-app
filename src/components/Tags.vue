<template>
	<div class="input__group">
		<small class="input__group--info">* You can add up to {{ maxTag }} tags </small>
		<div class="input__group--wrapper" id="wrapper">
			<transition-group name="slide-fade">
				<span class="input__group--span" v-for="(tag, index) in tags" v-bind:key="tag.key" :key="tag.id">
					<span class="input__group--value">
						{{ tag.value }}
					</span>
					<span class="input__group--close" @click="removeTag($event, index)"> X </span>
				</span>
			</transition-group>
			<input class="input__group--input"
						 @keydown.enter="valueCheck"
						 @keydown.tab.stop.prevent="valueCheck"
						 @keydown.backspace="removeTag($event, undefined)"
						 type="text"
						 placeholder="write tags, least 4 letters." id="tagInput">
		
		</div>
		<transition name="slide-fade">
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
			isErrorGroup: false,
			isDisabled : false,
			isError: false,
			maxTag: 7,
			minLetters: 4
		}
	},
	watch: {
		isErrorGroup: function () {
			let elm = document.getElementById("wrapper");
			if (this.isErrorGroup == true) {
				elm.classList.add('error-set')
			} else {
				elm.classList.remove('error-set')
			}
		},
		isDisabled: function () {
			let elmInput = document.getElementById('tagInput');
			if (this.isDisabled == true) {
				elmInput.disabled = true;
			} else {
				elmInput.disabled = false;
			}
		}
	},
	methods: {
		valueCheck(event) {
			let evenTarget = event.target;
			let spanCheck = document.getElementsByClassName("input__group--span");
			let validate = false;
			if (evenTarget.value.length >= this.minLetters) {
				this.tags.forEach(tagValue => {
					if (tagValue.value.toLowerCase() === evenTarget.value.toLowerCase()) {
						validate = true;
					}
				})
				if ( !validate) {
					if (spanCheck.length < this.maxTag) {
						this.isError = false;
						this.isErrorGroup = false;
						this.isDisabled = false;
						this.addTags(evenTarget)
					} else {
						this.isError = true;
						this.isErrorGroup = true;
						this.isDisabled = true;
						setTimeout(() => {
							this.isError = false;
							this.isErrorGroup = false;
						}, 3000)
					}
				} else {
					this.noAddTags();
				}
				evenTarget.value = ''
			}
		},
		addTags(event) {
			let id = this.tags.length + 1;
			this.tags.push({
				id: id,
				value: event.value
			});
			this.isVisible = false;
			this.isErrorGroup = false;
		},
		noAddTags() {
			this.isVisible = true;
			this.isErrorGroup = true;
			setTimeout(() => {
				this.isVisible = false;
				this.isErrorGroup = false;
			}, 3000)
		},
		removeTag(event, index) {
			if (index == undefined) {
				if (event.target.value <= 0) {
					this.tags.splice(this.tags.length - 1, 1)
					this.isDisabled = false;
				}
			} else {
				this.tags.splice(index, 1)
				this.isDisabled = false;
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
		color: red;
	}
	
	&--info {
		display: flex;
		font-size: 11px;
		color: #2d3e4f
	}
}

.error-set {
	animation-name: shakeError;
	animation-fill-mode: forwards;
	animation-duration: .6s;
	border: 1px solid red;
	animation-timing-function: ease-in-out;
}
.slide-fade-enter-active {
	transition: all .5s ease;
}

.slide-fade-leave-active {
	transition: all .5s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}

.slide-fade-enter, .slide-fade-leave-to {
	transform: translateX(10px);
	opacity: 0;
}

@keyframes shakeError {
	0% {
		transform: translateX(0);
	}
	15% {
		transform: translateX(0.375rem);
	}
	30% {
		transform: translateX(-0.375rem);
	}
	45% {
		transform: translateX(0.375rem);
	}
	60% {
		transform: translateX(-0.375rem);
	}
	75% {
		transform: translateX(0.375rem);
	}
	90% {
		transform: translateX(-0.375rem);
	}
	100% {
		transform: translateX(0);
	}
}
</style>
