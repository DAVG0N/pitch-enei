<script>
  export let preco = "";
  export let nome = "";
  export let publico = "";
  export let beneficios = [];
  export let cardType = "basico"; // basico, completo, etc.
  export let incompleto = false;
</script>

<div class="bilhete-card {cardType} {incompleto ? 'incompleto' : ''}">
  <div class="bilhete-header">
    <span class="bilhete-preco">{preco}</span>
    <h3>{nome}</h3>
  </div>
  <div class="bilhete-content">
    <p class="bilhete-publico">
      {publico}
    </p>
    <ul class="bilhete-beneficios">
      {#each beneficios as beneficio}
        <li class={beneficio.incluido === false ? 'nao-inclui' : (beneficio.incompleto ? 'incompleto' : '')}>
          {#if beneficio.incluido === false}
            <i class="fa-solid fa-xmark"></i>
          {:else if beneficio.incompleto}
            <i class="fa-solid fa-question"></i>
          {:else}
            <i class="fa-solid fa-check"></i>
          {/if}
          {beneficio.texto}
        </li>
      {/each}
    </ul>
    {#if incompleto}
      <span class="badge-incompleto">Em desenvolvimento</span>
    {/if}
  </div>
</div>
