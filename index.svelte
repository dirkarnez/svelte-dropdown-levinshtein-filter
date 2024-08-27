<script>
 	import { distance } from "fastest-levenshtein";

	let menuOpen = false;
	let inputValue = "";
	$:console.log(inputValue)
	
	const menuItems = ["About", "Base", "Blog", "Contact", "Custom", "Support", "Tools", "Boats", "Cars", "Bikes", "Sheds", "Billygoats", "Zebras", "Tennis Shoes", "New Zealand"];
	let filteredItems = [];
  
	const levenshtein_ratio = (s, t) => 1 - distance(s, t) / Math.max(s.length, t.length);

	const handleInput = () => {
	    const input = inputValue.toLowerCase();
	    filteredItems = menuItems.map(item => {
	      const content = item.toLowerCase();
	      const ratio = levenshtein_ratio(input, content);
	      return {
		content: content, 
		ratio: ratio
	      }
	    })
	    .filter(item => item.ratio >= 0.2)
	    .sort((a, b) => b.ratio - a.ratio)
	    .map(item => {
	      /* 
	      debugging
	      return `${item.content}, ${item.ratio}`
	      */
	      return item.content;
	    });
	}
</script>


<section class="dropdown">
	<input type="text" placeholder="Search..." autocomplete="off" id="searchInput" 
		bind:value={inputValue} 
		on:focus={() => menuOpen = true}
		on:focusout={() => menuOpen = false}
		{menuOpen} on:input={handleInput}>
		<span id="search-icon">🔍</span>
		{#if Array.isArray(filteredItems) && filteredItems.length > 0}
		  <div id="myDropdown" class:show={menuOpen} class="dropdown-content">		
				<!-- MENU -->
					{#each filteredItems as item}
						<div>{item}</div>
					{/each}
		  </div>	
		{/if}	
</section>
	
	
<style>		
.dropdown {
  position: relative;
  display: inline-block;
}
	
.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f6f6f6;
  min-width: 230px;
  border: 1px solid #ddd;
  z-index: 1;
}

/* Show the dropdown menu */	
.show {display:block;}	
</style>
