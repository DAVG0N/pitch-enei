<script>
  export let titulo = "";
  export let icon = "";
  export let mealType = ""; // pequeno-almoco, almoco, jantar
  export let descricao = "";
  export let opcoes = [];
  export let bebidas = "";
</script>

<div class="refeicao-card {mealType}">
  <div class="refeicao-header">
    <i class="fa-solid {icon}"></i>
    <h3>{titulo}</h3>
  </div>
  <div class="refeicao-content">
    {#if descricao}
      <p class="refeicao-descricao">{descricao}</p>
    {/if}

    {#if opcoes.length > 0}
      <div class="refeicao-opcoes">
        {#each opcoes as opcao}
          <div class="opcao-item">
            <i class="fa-solid {opcao.icon}"></i>
            <span>{opcao.texto}</span>
          </div>
        {/each}
      </div>
    {/if}

    {#if bebidas}
      <div class="bebidas">
        <i class="fa-solid fa-glass-water"></i>
        <span>{bebidas}</span>
      </div>
    {/if}
  </div>
</div>
