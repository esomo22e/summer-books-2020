<script>
	import { onMount } from 'svelte';
	import { digitFormat } from '../helpers/digitFormat.js'
	import { createEventDispatcher } from 'svelte';

	let el;

	export let data = {data};

let dataFiction = data.filter(function (currentElement) {
  // the current value is an object, so you can check on its properties
  return currentElement["Genre (Fiction, non-fiction, young adult, children)"] == "Fiction"
});


console.log(data);
// console.log(dataFilter);

</script>
<style>
#book-grid{
	width: 100%;
max-width: 80rem;
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

</style>
<div bind:this={el} class="content" id="content">
<div class="content-grid-item" id = "book-grid">
{#each dataFiction as books}

<!-- {console.log(books)} -->
<div id = "book-tile">
<div class = "img-cont">
<img src={"http://news.northeastern.edu/interactive/2021/06/summer-books/images/"+books["images"]+".png"}  alt={books["Book suggestion"]} class = "tile-image">

</div>
<!-- {console.log(books["images"])} -->
<!-- <img src = "http://news.northeastern.edu/interactive/2021/06/summer-books/images/corey.png" alt={books["Book suggestion"]}>  -->
<div id = "book-content">
<div id = "book-title"><a href="">{books["Book suggestion"]}</a></div>

<div id = "book-author">By {books["Author"]}</div>
<!-- <div id = "book-genre">{books["Genre (Fiction, non-fiction, young adult, children)"]}</div> -->

<div id = "book-desc">{books["Text of recommendation"]}</div>
<div id = "book-rec"> — {books["Recommended by"]}</div>
<div id = "info">{books["Northeastern affiliation of recommender"]}</div>

<div id = "book-att"></div>
</div>
</div>
{:else}
<!-- this block renders when photos.length === 0 -->
<p>loading...</p>
{/each}
	<!-- <h3>{dataFilter["Month"]} <div id = "changeNum" bind:this={container} /> </h3> -->

	<!-- <span class="brief-stat">{(data["Average Time on Page"]).toLocaleString('en-US')}</span> -->
	<!-- <span class="content-stat" id = "content-stat">{goalFilter["Month"]} goal is <span id = "goalNum">{goalFilter["Average Time on Page 2020"]} </span> </span> -->
	<!-- <span class="content-stat">{filterData} </span> -->

</div>
</div>
