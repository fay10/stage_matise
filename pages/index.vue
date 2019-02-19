<template>
	<div>
		<header v-for="(data, index) in page.articles" v-if="index === 0" :key="index" class="header">
			<div class="header__img">
				<img :src="data.image" alt="header-image" />
			</div>
			<div class="header__text">
				<h1>{{ data.title }}</h1>
				<h2>{{ data.subtitle }}</h2>
			</div>
		</header>
		<main class="container">
			<div v-for="(data, index) in page.articles" v-if="index !== 0" :key="index" class="box">
				<div :class="`box__block box__block--${data.color}`"></div>
				<div class="box__content">
					<h3 class="box__title">{{ data.title }}</h3>
					<h4 v-if="data.subtitle" class="box__subtitle">{{ data.subtitle }}</h4>
					<div class="content" v-html="data.content"></div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>
export default {
	components: {},
	data() {
		return {};
	},
	async asyncData({ app }) {
		const page = await app.$axios.$get('https://stage.matise.nl/api/');
		console.log(page);

		if (page) {
			return {
				page
			};
		}
	}
};
</script>

<style lang="scss">
@import '~tools';
.header {
	padding: grid(1 80/60 0);
	@media #{$small-only} {
		padding: 0;
	}
	&__img {
		width: 100%;
		height: rem(640);
		display: block;
		img {
			width: 100%;
			height: 100%;
			object-fit: cover;
			object-position: center;
		}
	}
	&__text {
		position: absolute;
		top: 40%;
		background-color: color(White);
		width: auto;
		padding: 40px 80px;
		@media #{$small-only} {
			position: static;
			background-color: black;
			padding: 30px;
			width: 100%;
			height: auto;
			color: color(White);
			line-height: 15px;
			padding: 40px;
		}
	}
}

.container {
	width: 100%;
	padding: 3.5rem grid(80/60);
	display: flex;
	flex-wrap: wrap;
	@media #{$large-down} {
		justify-content: space-between;
	}
	@media #{$small-only} {
		padding: 0;
		padding-top: 2rem;
	}
}

.box {
	flex-grow: 1;
	width: calc(100% / 3 - #{grid(2 / (1/3))});
	& + .box {
		margin-left: grid(80/60);
		@media #{$small-only} {
			margin-left: 0;
			margin-top: rem(40);
		}
	}
	@media #{$large-down} {
		flex-grow: unset;
		width: calc(50% - #{grid(0.5)});
		&:nth-of-type(3) {
			flex-grow: 1;
			margin-top: rem(40);
		}
		& + .box {
			margin-left: 0;
		}
	}

	@media #{$small-only} {
		width: calc(100% - 32px);
	}

	&:nth-child(3) {
		margin-bottom: 100px;
	}

	&:nth-of-type(2) {
		flex-grow: 2;

		@media #{$large-down} {
			flex-grow: unset;
		}
	}
	&__content {
		margin-top: 24px;

		@media #{$small-only} {
			padding-left: 0;
			padding-right: 30px;
		}
	}
	&:nth-child(odd) {
		@media #{$small-only} {
			margin-left: 30px;
		}
	}

	&__block {
		display: block;
		height: 260px;
		width: 100%;
		&--lime {
			background-color: color(Lime);
		}
		&--blue {
			background-color: color(Blue);
		}
		&--orange {
			background-color: color(Orange);
		}
		&--black {
			background-color: color(Black);
		}
	}
}
</style>
