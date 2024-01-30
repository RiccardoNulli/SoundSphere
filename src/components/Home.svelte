<script>
  import Header from "./Header.svelte";
</script>

<div class="container">
  <Header />
  <header>
    <h1>Benvenuti su SoundSphere</h1>
    <p>
      Esplora le nostre recensioni degli ultimi album e scopri nuova musica!
    </p>
  </header>
  <main>
    <section class="about">
      <h2>Chi Siamo</h2>
      <p>
        SoundSphere è una piattaforma dedicata agli appassionati di musica, dove
        potrai trovare molte cose belle.
      </p>
    </section>
    <section class="features">
      <h2>Cosa Offriamo</h2>
      <ul>
        <li>TI toccherà scoprirlo da solo/a :)</li>
      </ul>
    </section>
  </main>
  <footer>
    <p>&copy; 2024 SoundSphere. Tutti i diritti non riservati.</p>
  </footer>
</div>

<style>
  .container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }

  header {
    text-align: center;
    margin-bottom: 40px;
  }

  h1 {
    font-size: 2.5em;
    margin-bottom: 10px;
  }

  .about,
  .features {
    margin-bottom: 40px;
  }

  ul {
    list-style: none;
    padding: 0;
  }

  footer {
    text-align: center;
    margin-top: 40px;
  }

  footer p {
    font-size: 0.8em;
    color: #666;
  }
</style>
