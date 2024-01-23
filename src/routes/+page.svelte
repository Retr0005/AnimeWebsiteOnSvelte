
<script>
    import { onMount } from 'svelte';
  
    const delay = 1000 / 3;
    let firstCrumb =1;
    let SecondCrumb =2;
    let thirdCrumb =3;

    let animeList = [];
  
    function handleAnimeClick(animeId) {
      localStorage.setItem('selectedAnimeId', animeId);
      window.open('/details', '_blank');
    }
  
    function handleChangePage (page){
      animeList= [];
      if (page =="homepage"){
        firstCrumb = 1;
        SecondCrumb = 2;
        thirdCrumb = 3;
        page=firstCrumb;
      }
      else if (page=="goBack") {
        firstCrumb -=1;
        SecondCrumb -=1;
        thirdCrumb -=1;
        page=firstCrumb;
        console.log(page)
      }
      else if (page=="goForward") {
        firstCrumb +=1;
        SecondCrumb +=1;
        thirdCrumb +=1;
        page=firstCrumb;
        console.log(page)
      }
      else {
        firstCrumb = page;
        SecondCrumb = page +1;
        thirdCrumb = page +2;
      }
  
      setTimeout(async () => {
        try {
			const response = await fetch(`https://api.jikan.moe/v4/anime?order_by=popularity&page=${page}`);
  
          if (!response.ok) {
            return;
          }
          const data = await response.json();
		  console.log(data)
		  for (let index = 0; index < 25; index++) {

				const anime = {
            	id:data.data[index].mal_id,
            	imageUrl: data.data[index].images.jpg.image_url,
            	title: data.data[index].title_english,
          	};
			
			  animeList = [...animeList, anime];
			
		  }
          
  
        } catch (error) {
          console.error(`Error fetching anime ${i}:`, error);
        }
      });
    }
  
  
  onMount(() => {
    const interval = setInterval(nextImage, 5000); // Cambia immagine ogni 5000 millisecondi (5 secondi)

  
    for (let i = 0; i < 1; i++) {
      setTimeout(async () => {
        try {
			const response = await fetch(`https://api.jikan.moe/v4/anime?order_by=popularity`);
  
          if (!response.ok) {
            return;
          }
          const data = await response.json();
		  console.log(data)
		  for (let index = 0; index < 25; index++) {

				const anime = {
            	id:data.data[index].mal_id,
            	imageUrl: data.data[index].images.jpg.image_url,
            	title: data.data[index].title_english,
          	};
			
			  animeList = [...animeList, anime];
			
		  }
          
  
        } catch (error) {
          console.error(`Error fetching anime ${i}:`, error);
        }
      }, i * delay);
    }

    return () => clearInterval(interval); // Pulisce l'intervallo quando il componente viene distrutto

  });





  let currentImageIndex = 0;
  const images = [
    'https://www.animeunity.to/images/Sfondo2.png',

    /*
    'https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.freepik.com%2Ffree-photos-vectors%2Fbanner-anime%2F33&psig=AOvVaw0XJ8jBe-ISVGvgjg2bJOi6&ust=1706084945790000&source=images&cd=vfe&opi=89978449&ved=0CBAQjRxqFwoTCPDzv_mL84MDFQAAAAAdAAAAABAD',
    'https://i0.wp.com/wallpapercave.com/wp/wp3534292.jpg',
*/

  ];

  function nextImage() {
    currentImageIndex = (currentImageIndex + 1) % images.length;
  }

  function prevImage() {
    currentImageIndex = (currentImageIndex - 1 + images.length) % images.length;
  }

  
  </script>



<div class="immagine_iniziale">
  <button style=" position: absolute; top: 50%; left: 10px" class="btn btn-sm bg-indigo-600 bg-opacity-75 text-white text-opacity-75" on:click={prevImage} >&lt;</button>
  <img style="max-width: 100%; height: auto;" src={images[currentImageIndex]} alt="">
  <button style=" position: absolute; top: 50%; right: 10px" class="btn btn-sm bg-indigo-600 bg-opacity-75 text-white text-opacity-75" on:click={nextImage}>&gt;</button>
</div>



<div class="container flex">

<blockquote class="my-4 border-s-8 border-indigo-600 justify-center ml-44 my-8 ">
    <p class="font-sans text-7xl font-medium ">Trending anime</p>
</blockquote>
<p class="font-sans text-4xl font-medium my-16 ml-40 ">search:</p>
<input class="input max-h-12 ml-4 max-w-56 my-16" type="search" name="demo" placeholder="Search..." />
</div>





<div class="container flex flex-wrap justify-center items-center mx-auto gap-x-4 gap-y-4 mb-8 ">
	{#each animeList as { id, imageUrl, title}}
  <div class="box">
	<div class="card h-[432px] w-64">
		<header class="card-header  "><img class="max-h-[320px]" src="{imageUrl}"  on:click={() => handleAnimeClick(id)} alt=""></header>
		<section class="p-4 text-center font-bold max-h-[56px] ">{title}</section>
		<footer class="card-footer text-center font-semibold">streaming now!</footer>
	</div>
</div>

	{/each}
</div>
<div  class="">
	<ol class="breadcrumb flex justify-center mb-4">
    <li class="crumb"><button class="btn btn-sm bg-indigo-600 bg-opacity-75 text-white text-opacity-75" on:click={() => handleChangePage('homepage')} >&lt;&lt;</button></li>
		<li class="crumb"><button class="btn btn-sm bg-indigo-600 bg-opacity-75 text-white text-opacity-75" on:click={() => handleChangePage('goBack')} >&lt;</button></li>
		<li class="crumb"><button class="btn btn-sm bg-indigo-600 bg-opacity-75 text-white text-opacity-75" on:click={() => handleChangePage(firstCrumb)} >{firstCrumb}</button></li>
    <li class="crumb"><button class="btn btn-sm bg-indigo-600 bg-opacity-75 text-white text-opacity-75" on:click={() => handleChangePage(SecondCrumb)} >{SecondCrumb}</button></li>
		<li class="crumb"><button class="btn btn-sm bg-indigo-600 bg-opacity-75 text-white text-opacity-75" on:click={() => handleChangePage(thirdCrumb)} >{thirdCrumb}</button></li>
    <li class="crumb"><button class="btn btn-sm bg-indigo-600 bg-opacity-75 text-white text-opacity-75" on:click={() => handleChangePage('goForward')} >&gt;;</button></li>

  </ol>
</div>

