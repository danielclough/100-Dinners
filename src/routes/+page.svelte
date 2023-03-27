<script lang="ts">
  import recipes from '$lib/recipes.json';

  const parseAmount=(str: string)=>{
	let convert = "";
	let newStr: string[] = [];
	let split = str.split(" ");
	let reversed = split.reverse();
	for (let x of reversed) {
		if (/^-?[\d.]+(?:e-?\d+)?$/.test(x)) {
				newStr.push(String(parseInt(x) * multiplier))
		} else newStr.push(x)
	}
	return newStr.reverse().join(" ");
  }

  const ifTablespoons = (x: string, convert: string, newStr: string[]) => {
		if (x.includes("tablespoons")) {
			if (multiplier > 0) {
				convert = "tablespoons";
				newStr.push("cups")
			} else newStr.push(x)
		}
		else if (/^-?[\d.]+(?:e-?\d+)?$/.test(x)) {
			if (multiplier > 0 && !!convert) {
				newStr.push(String((parseInt(x) * multiplier) / 16 ))
			} else newStr.push(x)
		}
		else newStr.push(x)
  }

  const openHowMany = (recipe_name: string) => {
	howMany = true;
	active = recipe_name;
  }

  const doTheMulti = () => {
	multiplier = multiplierString;
	howMany = false;
  }

  $: multiplier = 0;
  $: multiplierString = 0;
  $: active = "";
  $: howMany = false;
</script>
 
<svelte:head>
  <title>Recipes</title>
</svelte:head>
 
<header class="heading">
  <div class="title">
    <h1>Recipes</h1>
  </div>
</header>
<main>
  <section>
      {#each recipes as { recipe_name, prep_time, cook_time, total_time, servings, total_yield, ingredients, directions, rating, url, cuisine_path, nutrition, timing, img_src }, index}
		<div class="recipe">
			{#if active == "" || active === recipe_name}
				<div class="img">
					<img src={img_src}  alt={recipe_name} on:click={() => openHowMany(recipe_name)}/>
					<h2>
						{recipe_name}
					</h2>
				</div>
			{/if}
			<article aria-posinset={index + 1} aria-setsize={recipes.length} class="person">
				{#key active == recipe_name}
				{#if active == recipe_name && multiplier > 0}
					<p><strong>{cook_time}</strong></p>
					{#if total_time}
						<p>
							Total Time: {total_time}
						</p>
					{/if}
					{#if prep_time}
						<p>
							Prep Time: {prep_time}
						</p>
					{/if}			
					<p>
						Servings: {servings * multiplier}
					</p>
					<p>
						Yield: {total_yield.split(" ").map((x)=> /^-?[\d.]+(?:e-?\d+)?$/.test(x) ? parseInt(x) * multiplier : x ).join(" ")}
					</p>
					<div>
						<h3>
							Ingredients:
						</h3>
						<ul>
							{#each ingredients.split("; ") as ingredient}
								<li class="clean">
								</li>
								<input type="checkbox" id={ingredient} name={ingredient} value={ingredient} >
								<label for={ingredient}>{parseAmount(ingredient)}</label>
							{/each}
						</ul>
					</div>
					<div>
						<h3>
							Directions:
						</h3>
						<ol>
							{#each directions.split(", ") as direction}
								<li>
									{direction}
								</li>
							{/each}
						</ol>
					</div>
					<div>
							{#each cuisine_path.split(" ") as tag}
								<span class="tag">
									#{tag}
								</span>
							{/each}
					</div>
					<div>
						<h3>
							Nutrition:
						</h3>
						<ul>
							{#each nutrition.split(", ") as fact}
								<li class="clean">
									{fact}
								</li>
							{/each}
						</ul>
					</div>
					<p>
						Timing: {timing}			
					</p>
				{/if}
				{/key}
			</article>
		</div>
      {/each}
  </section>
</main>
{#if howMany}
	<div class="how-many">
		<h2>
			How Many Meals?
		</h2>
		<input id="how-many" bind:value={multiplierString} />
		<button on:click={()=>doTheMulti()}>
			Go!
		</button>
	</div>
{/if}

<style>
	img {
		width: 50vw;
		padding-right: 2rem;
	}
	.img {
		display: flex;
	}
	.tag {
		padding: .5rem;
	}
	.clean {
		list-style: none;
	}
	#how-many {
		height: 2rem;
		width: calc(100% - 1rem)
	}
	.how-many {
		padding: 1rem;
		position: fixed;
		width: 90vw;
		height: 96vh;
		top: 2vh;
		left: 5vw;
		background-color: rgba(255,255,255,0.9);
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-content: center;
	}
</style>