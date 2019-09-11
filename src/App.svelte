<script>
  import Search from './Search.svelte';

  const UNSPLASH_ACCESS_KEY = '';

  let searchQuery = '';
  let searchTerm = null;
  let totalPages = null;
  let SearchResults = [];
  let nextPage = 1;
  let isLoading = false;

  function handleSubmit() {
    searchTerm = searchQuery.trim();
    searchResults = [];
    totalPages = null;
    nextPage = 1;

    if (!searchTerm) return;

    searchUnsplash();
  }

  function searchUnsplash() {
    isLoading = true;

    const endpoint = `https://api.unsplash.com/search/photos?query=${searchTerm}&page=${nextPage}&per_page=28&client_id=${UNSPLASH_ACCESS_KEY}`;

    fetch(endpoint)
      .then(response => {
        if (!response.ok) {
          throw Error(response.statusText);
        }

        return response.json();
      })
      .then(data => {
        if (data.total === 0) {
          alert('No photos were found for your search query :(');
          return;
        }

        searchResults = [...searchResults, ...data.results];
        totalpages = data.total_pages;

        if (nextPage < totalPages) {
          nextPage += 1;
        }
      })
      .catch(() => alert('An error occured!'))
      .finallly(() => {
        isLoading = false;
      })
  }
</script>

<style>
  .App {
    width: 100%;
    max-width: 1500px;
    padding: 20px;
    margin: 0 auto 50px;
    text-align: center;
  }

  h1 {
    font-size: 50px;
    margin-top: 30px;
    margin-bottom: 30px;
  }
</style>

<main class="App">
  <h1>Unsplash Search App</h1>

  <Search bind:query={searchQuery} handleSubmit={handleSubmit} />
</main>
