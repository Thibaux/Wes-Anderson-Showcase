<template>
	<div class="container">
		<div class="filmContent">
			<div class="filmContentHeader">
				<div class="top">
					<p class="releaseDate">{{ gbObject.release_date }}</p>
				</div>
				<div class="middle">
					<p class="">{{ revenue[0] }} </p>
					<p class="revenue"> revenue </p>
					<p> - {{ gbObject.runtime }} minutes </p>
				</div>
				<div class="bottom">
					<div class="genres" v-for="genre in genres">
						<p>{{ genre }}</p>
					</div>
				</div>
				<p class="overview">{{ gbObject.overview }}</p>
			</div>
		</div>
		<div tabindex="-1" id="mk" class="filmContent">
			<h3>{{ gbObject.title }}</h3>
			<p class="sText tagline">{{ gbObject.tagline }}</p>
			<div class="actors">
				<div class="actor"><img :src="GBActor1" alt=""></div>
				<div class="actor"><img :src="GBActor2" alt=""></div>
				<div class="actor"><img :src="GBActor3" alt=""></div>
				<div class="actor"><img :src="GBActor4" alt=""></div>
				<div class="actor"><img :src="GBActor5" alt=""></div>
				<div class="actor"><img :src="GBActor6" alt=""></div>
			</div>
		</div>
		<div class="poster">
			<img :src="GBPoster" alt="poster">
		</div>
	</div>
</template>
<script>
import * as HTTP from './../../../http-common';
var numeral = require("numeral");

export default {
	data() {
		return {
			gbObject: {},
			GBPoster: '',
			GBActor1: '',
			GBActor2: '',
			GBActor3: '',
			GBActor4: '',
			GBActor5: '',
			GBActor6: '',
			revenue: [],
			genres: [],
		};
	},
	beforeMount() {
		this.fetchBeforePageLoads();
	},
	methods: {
		fetchBeforePageLoads() {
			this.fetchGb();
		},
		// Fetch Grand budapest
		fetchGb() {
			HTTP.HTTP.get(`movie/120467?api_key=${process.env.VUE_APP_API_KEY}`)
				.then(response => {
					this.gbObject = response.data
					this.GBPoster = "https://image.tmdb.org/t/p/w500/" + `${this.gbObject.poster_path}`
					this.revenueConverter()
					this.fetchCredits()
					this.pushGenresIntoArray()
				})
				.catch(error => {
					console.log(error)
					this.showErr = true;
				})
		},
		revenueConverter() {
			let revenue = []
			revenue.push(this.gbObject.revenue)

			for (let i = 0; i < 7; i++) {
				this.revenue.push(numeral(revenue[i]).format('($ 0.00 a)'));
			}
		},
		fetchCredits() {
			HTTP.HTTP.get(`movie/120467/credits?api_key=${process.env.VUE_APP_API_KEY}`)
				.then(response => {
					this.gbCreditsObject = response.data
					console.log(this.gbCreditsObject)
				})
				.catch(error => {
					console.log(error)
					this.showErr = true;
				})
		},
		pushGenresIntoArray() {

			for (let i = 0; this.gbObject.genres.length; i++) {

				this.genres.push(this.gbObject.genres[i].name)
			}
		},
	}
};
</script>
<style lang="scss" scoped>
@import "./../../assets/main.scss";

.container {
	display: flex;
	flex-direction: row;
	gap: 1rem;
	margin-top: 1rem;
	width: 100%;

	img {
		width: 60%;
		height: auto;
		border-radius: 8px;
		box-shadow: 0 2.8px 2.2px rgba(0, 0, 0, 0.034), 0 6.7px 5.3px rgba(0, 0, 0, 0.048), 0 12.5px 10px rgba(0, 0, 0, 0.06), 0 22.3px 17.9px rgba(0, 0, 0, 0.072), 0 41.8px 33.4px rgba(0, 0, 0, 0.086), 0 100px 80px rgba(0, 0, 0, 0.12);
	}

	.filmContent {
		width: 25rem;
		display: flex;
		flex-direction: column;

		h3 {
			font-family: $font-family-title;
			font-size: 35px;
			margin-top: 0.5rem;
		}

		.tagline {
			font-size: $font-size-stext;
			font-style: italic;
		}

		.rDate {
			font-size: 25px;
		}


	}

	.filmContent:nth-child(2) {
		width: 35rem;
	}

	.filmContentHeader {
		display: flex;
		flex-direction: column;
		margin-left: 10%;
		align-items: left;


		.top {
			display: flex;
			flex-direction: column;

			.releaseDate {
				margin-left: -40%;
				margin-bottom: 1rem;
			}
		}

		.middle {
			display: flex;
			flex-direction: row;
			font-size: $font-size-stext;
			margin-bottom: 0.25rem;

			.revenue {
				margin-left: 0.4rem;
				margin-right: 0.4rem;
			}
		}


		.bottom {
			display: flex;
			flex-direction: row;
			font-size: $font-size-stext;

			p {
				margin-right: 0.4rem;
			}
		}

		.overview {
			font-size: $font-size-stext;
			margin-top: 1rem;
			margin-left: -3rem;
		}
	}
}
</style>