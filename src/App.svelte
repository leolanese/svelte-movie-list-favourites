<script>
  import MovieItem from "./Movie/Item.svelte";

  const APIKEY = "b25196cb6ea1946dc9e8fd6c3ddbe0a9";
  const BASEURL = "https://api.themoviedb.org/3";
  const APISETTINGS = `?api_key=${APIKEY}&languages=en-UK`;

  // return Promise movies
  const movies = (async () => {
    const URL = `${BASEURL}/discover/movie${APISETTINGS}&sort_by=pupularity.asc`;
    const response = await fetch(URL);
    return await response.json();
  })();

  let favouriteMovies = [];

  function toggleLike(e) {
    console.log("toggleLike", e);

    const movie = e.detail;
    // add movie to favourites
    let index = favouriteMovies.findIndex((movie) => movie.id === movie.id);
    // index >= 0, we already have that movie in the list
    if (index >= 0) {
      favouriteMovies.splice(index, 1);
      console.log("favouriteMovies if", favouriteMovies);
    } else {
      console.log("favouriteMovies push", movie);
      favouriteMovies.push(movie);
    }
    favouriteMovies = favouriteMovies;
  }

  $: like = (id) => {
    let index = favouriteMovies.findIndex((movie) => movie.id === id);
    return index >= 0;
  };
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<svelte:head>
  <title>Svelte - Movie Db API</title>
  <link
    rel="stylesheet"
    href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css"
    integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z"
    crossorigin="anonymous" />
</svelte:head>

<main class="container">
  <div class="row">
    <div class="col-12 col-md-6 col-lg-8">
      <h2>Movies</h2>
      <!-- Parent using the movie child Component -->
      {#await movies}
        <div class="col-12 col-md-6 col-lg-3">LOADING ....</div>
      {:then data}
        {@debug data}
        {#each data.results as movie}
          <MovieItem
            id={movie.id}
            title={movie.title}
            overview={movie.overview}
            poster={movie.poster_path}
            like={like(movie.id)}
            on:onToggleLike={toggleLike} />
        {/each}
      {/await}
    </div>

    <div class="col-12 col-md-6 col-lg-4">
      <h2>Favourite Movies</h2>
      <div class="row">
        {#if favouriteMovies.length}
          {#each favouriteMovies as movie}
            <div class="col-12 col-md-6 col-lg-4 p-2">
              <MovieItem
                id={movie.id}
                title={movie.title}
                overview={movie.overview}
                poster={movie.poster}
                like={like(movie.id)}
                on:onToggleLike={toggleLike} />
            </div>
          {/each}
        {:else}
          <div class="col-12 col-md-6 col-lg-4">
            <h2>No favourites movies</h2>
            {favouriteMovies}
          </div>
        {/if}

      </div>
    </div>
  </div>
</main>
