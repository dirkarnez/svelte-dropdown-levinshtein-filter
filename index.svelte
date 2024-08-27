<script>
	let menuOpen = false;
	let inputValue = "";
	$:console.log(inputValue)
	
	const menuItems = ["About", "Base", "Blog", "Contact", "Custom", "Support", "Tools", "Boats", "Cars", "Bikes", "Sheds", "Billygoats", "Zebras", "Tennis Shoes", "New Zealand"];
	let filteredItems = [];
	// 
	const handleInput = () => {
		return filteredItems = !!inputValue ? 
			menuItems.filter(item => item.toLowerCase().match(inputValue.toLowerCase()))
			:
			[];	
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
