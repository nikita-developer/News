<script setup lang="ts">
const props = defineProps({
	person: {
		type: Object,
	},
})

const info = ref()
const is_loading = shallowRef(false)

const getInfo = async () => {
	is_loading.value = true

	try {
		const res = await fetch(`https://cdnapi.smotrim.ru/api/v1/persons/${props.person?.id}`)
		info.value = await res.json()
		is_loading.value = false
	} catch (e) {
		console.log(e)
	} finally {
		is_loading.value = false
	}
}

onBeforeMount(getInfo)

</script>

<template>
	<div class="user-info">
		<div class="user-info__outside" @click="$emit('close')"></div>
		<div class="user-info__body">
			<div class="user-info__preload" v-if="is_loading">
				<preload></preload>
			</div>
			<div class="user-info__close" @click="$emit('close')">
				<img src="@/assets/media/close.svg" alt="Закрыть">
			</div>
			<div class="user-info__content">
				<figure class="user-info__face">
					<div class="user-info__media">
						<img class="user-info__img"
						     :src="`https://api.smotrim.ru/api/v1/pictures/${props.person?.picId}/bq/redirect`" alt=""/>
					</div>
					<figcaption class="user-info__name">{{ props.person.name }} <span>{{ props.person.surname }}</span>
					</figcaption>
				</figure>
				<div class="user-info__text" v-html="info?.data?.body"></div>
			</div>
		</div>
	</div>
</template>

<style scoped lang="scss">
.user-info {
	position: fixed;
	inset: 0;
	display: flex;
	justify-content: center;
	align-items: center;
	background-color: hwb(0deg 50.2% 49.8% / 36%);
	backdrop-filter: blur(5px);
	padding: 15px;

	&__outside {
		position: absolute;
		inset: 0;
	}

	&__face {
		display: flex;
		column-gap: 32px;
		align-items: center;
		margin-bottom: 40px;

		span {
			display: block;
		}

		@media (max-width: 576px) {
			margin-bottom: 20px;
			column-gap: 20px;
		}
	}

	&__name {
		font-size: 40px;
		font-weight: 700;

		@media (max-width: 576px) {
			font-size: 28px;
		}
	}

	&__media {
		width: 200px;
		height: 200px;
		border-radius: 8px;
		background-color: gray;
		overflow: hidden;
		flex-shrink: 0;

		@media (max-width: 576px) {
			width: 150px;
			height: 150px;
		}
	}

	&__img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	&__body {
		position: relative;
		width: 100%;
		max-width: 605px;
		min-height: 600px;
		background-color: #fff;
		padding: 30px;
		border-radius: 16px;
		max-height: 100%;
		overflow: hidden;
	}

	&__preload {
		position: absolute;
		inset: 0;
		background-color: #fff;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	&__close {
		position: absolute;
		top: 15px;
		right: 15px;
		width: 32px;
		height: 32px;
		display: flex;
		justify-content: center;
		align-items: center;
		cursor: pointer;
	}

	&__text {
		display: flex;
		flex-direction: column;
		row-gap: 20px;
		overflow: auto;
		max-height: 300px;
		padding-right: 15px;

		p {
			font-size: 16px;
			font-weight: 400;
			color: rgba(0, 0, 0, 1);
		}
	}
}
</style>
