<template>
    <div v-if="countries">
		<div class="filters">
			<form class="search" @submit.prevent="">
				<input 
					type="text" 
					v-model="search_term"
					placeholder="Search for a country..."
				>
				<button type="submit"><span class="sr-only">Search</span></button>
			</form>

			<select v-model="filter_region">
				<option value="" disabled selected hidden>Filter by Region</option>
				<option>All</option>
				<option>Africa</option>
				<option>Americas</option>
				<option>Asia</option>
				<option>Europe</option>
				<option>Oceania</option>
			</select>
		</div>

		<div class="countries">
			<a v-for="(country, index) in filteredCountries"
				:key="index"
				:href="`/${country.name.official.replaceAll(' ', '-').toLowerCase()}`"
			>
				<article class="card">
					<div class="image-container">
						<img :src="country.flags.png" alt="">
					</div>
					<div class="card-content">
						<h2>{{ country.name.common }}</h2>
						<p><strong>Population:</strong> {{ country.population }}</p>
						<p><strong>Region:</strong> {{ country.region }}</p>
						<p><strong>Capital:</strong> {{ country.capital && country.capital[0] }}</p>
					</div>
				</article>
			</a>
		</div>

    </div>
</template>

<script lang="ts">
export default {
	props: {
		countries: {
			type: Object,
			required: true,
		}
	},
	computed: {
		filteredCountries() {
			let return_countries = this.countries;
			if (this.search_term.length > 2) {
				return_countries = return_countries.filter(country => country.name.official.toLowerCase().includes(this.search_term.toLowerCase()));
			}
			if (this.filter_region && this.filter_region !== "All") {
				return_countries = return_countries.filter(country => country.region === this.filter_region);
			}
			return return_countries;
		}
	},
	data() {
		return {
			search_term: '',
			filter_region: '',
		}
	},
}
</script>

<style lang="scss" scoped>
	@import "../styles/global.scss";

	form.search {
		position: relative;

		@media (min-width: $break-point) {
			width: 45%;
			max-width: 30rem;
		}

		input {
			display: block;
			width: 100%;

			font-size: 12px;

			background-color: var(--clr-elements);
			border: none;
			color: var(--clr-input);

			padding: 1.1rem 1rem;
			padding-inline-start: 4.5rem;
			border-radius: 0.5rem;

			box-shadow: 0 0 1.5rem -1.2rem black;

			&:focus-visible {
				outline-color: var(--clr-text);
			}
		}

		button {
			position: absolute;
			left: 2rem;
			top: 1rem;

			width: 16px;
			aspect-ratio: 1;

			border: none;
			background-color: var(--clr-input);
			-webkit-mask: url("/images/magnifying-glass-solid.svg") no-repeat center;
            mask: url("/images/magnifying-glass-solid.svg") no-repeat center;

			cursor: pointer;

			&:hover,
			&:focus-visible {
				background-color: var(--clr-text);
			}

			&:focus-visible {
				outline-color: var(--clr-text);
				outline-offset: 0.5rem;
			}
		}
	}

	.card {
		border-radius: 0.5rem;
		overflow: hidden;

		color: var(--clr-text);
		background-color: var(--clr-elements);

		margin-block-start: 2.5rem;

		box-shadow: 0 0 1.5rem -1rem black;

		@media (min-width: $break-point) {
			height: 100%;
			margin-block-start: 0;
		}
	}

	.card h2 {
		font-size: 18px;
		font-weight: 800;

		margin-block-end: 1rem;
	}

	.card p {
		font-size: 14px;
		line-height: 1.7;
	}

	.card p strong {
		font-weight: 600;
	}

	.card-content {
		padding: 1.2rem 1.5rem 3rem;
	}

	.countries {
		margin-inline: 2.5rem;

		@media (min-width: $break-point) {
			margin-inline: 0;
			margin-block-start: 2.5rem;

			display: grid;
			grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
			grid-gap: 4rem;
		}
	}

	.filters {
		@media (min-width: $break-point) {
			display: flex;
			justify-content: space-between;
			margin-block-start: 1rem;

			select {
				margin-block-start: 0;
			}
		}
	}

	.image-container > img {
		width: 100%;
		aspect-ratio: 4 / 2;
	}

	/* Styles for Custom Select Item */
	select {
		appearance: none;

		background-color: var(--clr-elements);
		border: none;
		color: var(--clr-input);

		padding: 1rem 1rem;
		border-radius: 0.5rem;

		box-shadow: 0 0 1.5rem -1.2rem black;

		margin-top: 2.5rem;

		/* change the arrow */
		padding-right: 3rem;
		background-image: url("/images/chevron-left-solid.svg");
		background-repeat: no-repeat;
		background-size: 10px;
		background-position: 90% 50%;

		&:focus-visible {
			outline-color: var(--clr-text);
		}

		&::-ms-expand {
			background-image: url("/images/chevron-down-solid.svg");
		}
	}
</style>