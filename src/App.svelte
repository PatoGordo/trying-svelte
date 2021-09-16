<script lang="ts">
  import axios from 'axios';

  type ShibaItem = {
    _id: string;
    url: string;
    createdAt: string;
    updatedAt: string;
    __v: number;
  }

  type IShibaReturn = {
    images: ShibaItem[]
  }

  let patos = [
    'Josefino',
    'Paulão do Trap',
    'Gordo',
    'Carlos José',
    'Dripado',
    'Vendedor de Drogas Usadas'
  ]

  async function getShibas() {
    const res = await axios.get('https://happy-api-nohorny.vercel.app/shiba/all')
    return (res.data) as IShibaReturn
  }
</script>

<main>
  <h1>Testing Svelte</h1>
  <br>
  <h2>Duck Family</h2>
  <ul>
    {#each patos as pato}
      <li>🦆 Pato {pato}</li>
    {/each}
  </ul>

  <br>
  <h2>Cute shibas</h2>
  <br>
  {#await getShibas()}
    <strong>Getting data please wait...</strong>
  {:then shibas}
    <div class="shibas-container">
      {#each shibas.images as shiba}
        <img class="lazyload" width="220" height="220" src={shiba.url} alt={shiba._id} />
      {/each}
    </div>
  {/await}
</main>

<style lang="scss" scoped>
  * {
    font-family: 'Roboto', sans-serif;
    margin: 0;
    padding: 0;
    list-style: none;
  }
  main {
    display: grid;
    place-items: center;
  }
  .shibas-container {
    display: grid;
    place-items: start;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 16px;
    margin-top: 16px;
    img {
      width: 220px;
      height: 220px;
      background-color: #ccc;
      object-fit: cover;
      border-radius: 16px;
      animation: SkeletonLoading infinite 1s;
    }
  }

  @keyframes SkeletonLoading {
    0% {
      background: #ccc;
    }
    50% {
      background: #ddd;
    }
    100% {
      background: #ccc;
    }
  }

  @media screen and (max-width: 715px) {
    .shibas-container {
      grid-template-columns: 1fr 1fr;
    }
  }
  @media screen and (max-width: 475px) {
    .shibas-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      img {
        width: 100%;
        height: 320px;
      }
    }
  }
</style>
