<script>
  import { Router, Link, Route } from "svelte-routing";
  import Home from "./components/Home.svelte";
  import UpcomingAlbums from "./components/upcoming-albums.svelte";
  import YourFavoriteAlbums from "./components/your-favorite-albums.svelte";
  import YourReviews from "./components/your-reviews.svelte";
  import HottestHits from "./components/Hottest-Hits.svelte";
  import "./vars.css";
  import "./styles.css";
</script>

<div>
  <!-- Meccanismo di routing delle pagine -->
  <Router>
    <Route path="/" component={Home} />
    <Route path="/Hottest-Hits" component={HottestHits} />
    <Route path="/upcoming-albums" component={UpcomingAlbums} />
    <Route path="/your-reviews" component={YourReviews} />
    <Route path="/your-favorite-albums" component={YourFavoriteAlbums} />
  </Router>
</div>

<style>
</style>
