<script setup lang="ts">
const props = defineProps({
	person: {
		type: Object,
	},
})
</script>

<template>
	<figure class="slider-item">
		<div class="slider-item__media">
			<img class="slider-item__img" :src="`https://api.smotrim.ru/api/v1/pictures/${props.person?.picId}/bq/redirect`" alt=""/>
		</div>
		<figcaption class="slider-item__name">{{ props.person.name }} <span>{{ props.person.surname }}</span></figcaption>
	</figure>
</template>

<style scoped lang="scss">
.slider-item {
	display: flex;
	flex-direction: column;
	align-items: center;

	&__media {
		width: 144px;
		height: 144px;
		border-radius: 50%;
		background-color: gray;
		margin-bottom: 6px;
		overflow: hidden;
	}

	&__img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	&__name {
		text-align: center;

		span {
			display: block;
		}
	}
}
</style>
