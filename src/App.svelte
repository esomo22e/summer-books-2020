<script>
	import { onMount } from 'svelte';

	import BarChart from './charts/BarChart.svelte'
	import DotPlot from './charts/DotPlot.svelte'
	import LineChart from './charts/LineChart.svelte'
	import LollipopChart from './charts/LollipopChart.svelte'
	import MapUSA from './charts/MapUSA.svelte'
	import Scatter from './charts/Scatter.svelte'
	import WaffleChart from './charts/WaffleChart.svelte'
	import XYHeatmap from './charts/XYHeatmap.svelte'
	import GraphicTitle from './components/GraphicTitle.svelte'
	import GraphicFooter from './components/GraphicFooter.svelte'
	import * as colors from './helpers/colors.js'
	import * as turnout from '../public/datasets/turnout.json'
	import * as applemaps from '../public/datasets/applemaps.json'
	import * as cannabislaws from '../public/datasets/cannabislaws.json'
	import * as waffledata from '../public/datasets/waffledata.json'

	import Fiction from './components/Fiction.svelte'
	import NonFiction from './components/NonFiction.svelte'

	import SvelteTable from "svelte-table"
import { csv, json } from 'd3-fetch'
// import { groups } from 'd3-array'
import { timeParse, timeFormat } from 'd3-time-format';

const todaysDate = new Date();
const dateCode = todaysDate.getTime();

	export let width = Math.min(
		document.getElementById('interactive').getBoundingClientRect().width,
		1000
	);

	// console.log(waffledata.default[0])

	export let height = 350;

	function getorientation(w) {
		if (w > 750) {
			return "vertical"
		} else {
			return "horizontal"
		}
	}
	const REMOTE_BASE_DIR = "//" + window.location.hostname + "/interactive/2021/06/summer-books/";
	 const url = REMOTE_BASE_DIR + "datasets/summer_books.json";

	// const url = 'https://spreadsheets.google.com/feeds/cells/1Ght4HV6yO9r0pWG3IP8aMyiDuCBjnF1OvjTMpyu1gIQ/1/public/full?alt=json&date=' + dateCode

	$: bookData = [];

	// NEED THESE TO CYCLE THROUGH THE HEADERS OF THE GOOGLE SHEET
	// const headings = ["Month", "Total page views 2020", "Total page views 2021", "Unique page views 2020", "Unique page views 2021", "Average Time on Page 2020", "Average Time on Page 2021",
	// "Acquisition by Source (Organic Search) 2020", "Acquisition by Source (Organic Search) 2021", "Email Open Rate 2020", "Email Open Rate 2021", "Email clicks 2020",  "Email clicks 2021" ]
// 	const headings = ["Book suggestion", "Author", "Genre (Fiction, non-fiction, young adult, children)", "Pub year", "Text of recommendation", "Recommended by", "Northeastern affiliation of recommender",  "images", "Goodreads link"]
//
	// THIS ACCESSES AND PROCESSES THE GOOGLE SHEET"
json(url).then(function(data,i){
	let rowcount = ((data.feed.entry.length / headings.length)-1)
	let loadeddata = []

	for (let r=0; r < rowcount; r++) {
		loadeddata[r] = {}
	}

	data.feed.entry.filter(d => (d.gs$cell.row !== "1")).forEach(function(d,i){
		let colno = parseFloat([d.gs$cell.col])-1

		if (colno === 0) {
			// loadeddata[parseFloat([d.gs$cell.row])-2][headings[colno]] = formatDate(parseTime(d.gs$cell.inputValue))
			loadeddata[parseFloat([d.gs$cell.row])-2][headings[colno]] = d.gs$cell.inputValue

		} else  {
			loadeddata[parseFloat([d.gs$cell.row])-2][headings[colno]] = d.gs$cell.inputValue
		}
		// else {
		// 	loadeddata[parseFloat([d.gs$cell.row])-2][headings[colno]] = parseFloat(d.gs$cell.numericValue)
		// }
	})

	bookData = loadeddata;


	// bookNF = loadeddata

	})

	// csv("../public/datasets/summerbooks.csv").then(function(data,i){
// csv("//news.northeastern.edu/interactive/2021/06/summer-books/datasets/summerbooks.csv").then(function(data,i){
// 	data.forEach(function(d,i){
// 		Object.keys(d).forEach(function(j) {
// 			console.log(d);
// 			// if ((j == "Date") || (j == "Mass. Positive Rate")) {
// 				d[j] = d[j]
// 			// } else {
// 			// 	d[j] = parseFloat(d[j])
// 			// }
//
// 		})
// 	});
//
// 	bookData = data;
//
//
// });


// console.log(bookNF)

</script>

<style>

#book-grid{
	/* width: 100%;
max-width: 80rem; */
width: 100%;
max-width: 100rem;
margin: 2em auto 0 auto;
	display:grid;
	grid-template-columns: repeat(2, 0.5fr);
	/* grid-gap: 4px; */
	/* padding: 0 200px  */
}

@media only screen and (max-width: 900px) {

	#book-grid{
		width: 90%;
		max-width: 90%;
		grid-template-columns: repeat(1, 1fr);
		margin: 0;
	}


}


 /* img {
		width: 100%;
		margin: 0;
	} */

</style>
{#if bookData.length > 0}

	<!-- <h1>Summer Books</h1> -->
<!-- {console.log(analyticData)} -->
<!-- <GraphicTitle
		title={"Non-Fiction"}
	/> -->
	<!-- <NonFiction
		data={bookData}

	/> -->

<!-- <GraphicTitle
		title={"Fiction"}
	/> -->
<!-- <div id="book-grid"> -->
<!-- <Fiction
	data={bookData}

/> -->
<div class="content-grid-item" id = "book-grid">

		{#each bookData as books}


		<div id = "book-tile">
		<div class = "img-cont">
		<img src={"http://news.northeastern.edu/interactive/2021/06/summer-books/images2/"+books["images"]+".png"}  alt={books["Book suggestion"]} class = "tile-image">

		</div>
		<!-- {console.log(books["images"])} -->
		<!-- <img src = "http://news.northeastern.edu/interactive/2021/06/summer-books/images/corey.png" alt={books["Book suggestion"]}>  -->
		<div id = "book-content">

		<div id = "book-title"><a href="{books["Goodreads link"]}" target="_blank">{books["Book suggestion"]}</a></div>

		<div id = "book-author">By {books["Author"]}</div>
		<!-- <div id = "book-genre">{books["Genre (Fiction, non-fiction, young adult, children)"]}</div> -->

		<div id = "book-genre">{books["Genre (Fiction, non-fiction, young adult, children)"]}</div>
		<div id = "book-desc">{books["Text of recommendation"]}</div>
		<div id = "recommender-cont">
		<div id = "book-rec"> — {books["Recommended by"]}</div>
		<div id = "book-rec-info">{books["Northeastern affiliation of recommender"]}</div>
		</div>
		<!-- <div id = "book-att"></div> -->
		</div>
		</div>
		{:else}
	<!-- this block renders when photos.length === 0 -->
 <p>loading...</p>
		{/each}
		</div>

<!-- </div> -->

{/if}





<!-- {console.log(analyticData)} -->
<!-- <GraphicFooter
	source={'<a href="https://electproject.github.io/Early-Vote-2020G/index.html">United States Elections Project</a>'}
	note={'Accessed Nov 2020'}
	credit={'Developer Credit/Northeastern University'}
/> -->
