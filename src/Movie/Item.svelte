<script>
  import { createEventDispatcher } from "svelte";

  // props
  export let id;
  export let title;
  export let overview;
  export let poster;
  export let like;

  // computer property
  $: coverURL = `https://image.tmdb.org/t/p/w185_and_h278_bestv2${poster}`;

  const dispatch = createEventDispatcher();

  function toggleLike(event) {
    dispatch("onToggleLike", { id, title, overview, poster });
    console.log("onLike", { id, title, overview, poster });
  }
</script>

<div class="card">
  <img class="card-img-top" src={coverURL} alt="Poster" />
  <div class="card-body">
    <h5 class="card-title">{title}</h5>
    <p class="card-text">{overview}</p>
    <div class="text-center">
      <!-- Child send dispatch -->
      <button class="btn btn-primary" on:click={toggleLike}>{like}</button>
    </div>
  </div>
</div>
