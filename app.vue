<script setup>
import {useWindowSize} from '@vueuse/core'

const {width} = useWindowSize()
const is_open = ref(false)
const persons = ref([])
const person = ref()
const is_loading = shallowRef(false)
const count_slide = shallowRef(8)

count_slide.value = computed(() => {
	if (width.value < 480) return 2
	if (width.value < 768) return 3
	if (width.value < 980) return 4
	if (width.value < 1512) return 6
	return 8
})

const getPersons = async () => {
	is_loading.value = true

	try {
		const res = await fetch('https://cdnapi.smotrim.ru/api/v1/boxes/vesti2')
		const json = await res.json()
		persons.value = json?.data?.content?.find(el => el.title === 'Персоны').content
		is_loading.value = false
	} catch (e) {
		console.log(e)
	} finally {
		is_loading.value = false
	}
}
const openInfo = (data) => {
	person.value = data
	is_open.value = true
}

onBeforeMount(getPersons)
</script>

<template>
	<div class="page">
		<div class="container">
			<div class="page__body">
				<div class="page__preloader" v-if="is_loading">
					<preload></preload>
				</div>
				<div class="slider">
					<carousel
							:itemsToShow="count_slide"
							:wrapAround="true"
							snapAlign="left"
					>
						<slide v-for="person in persons" :key="person.id">
							<slider-item
									class="slider__item"
									:key="person.id"
									:person
									@click="openInfo(person)"
							></slider-item>
						</slide>

						<template #addons>
							<navigation/>
						</template>
					</carousel>
				</div>
				<user-info
						v-if="is_open"
						:person
						@close="is_open = false"
				></user-info>
			</div>
		</div>
	</div>
</template>

<style lang="scss">
.page {
	display: flex;
	height: 100vh;
	width: 100vw;

	&__preloader {
		position: fixed;
		inset: 0;
		z-index: 999;
		background-color: #fff;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	&__body {
		width: 100%;
	}
}

.slider {
	&__item {
		width: 144px;
	}

	.carousel__prev {
		width: 48px;
		height: 48px;
		border-radius: 50%;
		background-image: url('@/assets/media/next.svg');
		background-position: center;
		transform: rotate(180deg) translateY(50%);
		background-color: #fff;
		background-repeat: no-repeat;
		top: 72px;
		left: -5px;
		margin: 0;

		svg {
			display: none;
		}
	}

	.carousel__next {
		width: 48px;
		height: 48px;
		border-radius: 50%;
		background-image: url('@/assets/media/next.svg');
		background-position: center;
		background-color: #fff;
		background-repeat: no-repeat;
		top: 72px;
		right: -5px;
		margin: 0;

		svg {
			display: none;
		}
	}
}

.container {
	display: flex;
	align-items: center;
	width: 100%;
	margin-right: auto;
	margin-left: auto;
	max-width: 1406px;
}
</style>
