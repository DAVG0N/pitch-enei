<script>
  import { onMount } from 'svelte';
  import Slide from './components/Slide.svelte';
  import TicketCard from './components/TicketCard.svelte';
  import MealCard from './components/MealCard.svelte';

  let currentSlide = 0;
  const totalSlides = 12;

  // Lista dos títulos dos slides para a barra inferior e navegação
  const slideTitles = [
    "Capa",
    "O Que é o ENEI?",
    "Pilares Estratégicos",
    "Empresas Parceiras",
    "Organização e Comissão",
    "Infraestruturas",
    "Plano de Execução",
    "Calendário",
    "Alimentação",
    "Planos de Bilhetes",
    "Roadmap",
    "Contacto"
  ];

  // Empresas de edições anteriores (Sponsors Históricos)
  const pastSponsors = [
    { nome: "Uphold (Gold Sponsor)", logo: "/assets/sponsors/gold/uphold.svg", cor: "#EFBF04" },
    { nome: "McDigital (Silver Sponsor)", logo: "/assets/sponsors/silver/mcdigital.png", cor: "#C4C4C4" },
    { nome: "NVIDIA", logo: "/assets/speakers/nvidia.svg", cor: "#76B900" },
    { nome: "RetailConsult (Gold Sponsor)", logo: "/assets/sponsors/gold/retailconsult.svg", cor: "#EFBF04" },
    { nome: "Checkmarx (Silver Sponsor)", logo: "/assets/sponsors/silver/checkmarx.svg", cor: "#C4C4C4" },
    { nome: "SupaBase", logo: "/assets/speakers/supabase.svg", cor: "#3ECF8E" },
    { nome: "Agentif AI (Gold Sponsor)", logo: "/assets/sponsors/gold/agentifai.svg", cor: "#EFBF04" },
    { nome: "Deloitte (Silver Sponsor)", logo: "/assets/sponsors/silver/deloitte.svg", cor: "#C4C4C4" },
    { nome: "Google", logo: "/assets/speakers/google.svg", cor: "#4285F4" },
    { nome: "Accenture (Gold Sponsor)", logo: "/assets/sponsors/gold/accenture.svg", cor: "#EFBF04" },
    { nome: "Glintt Global (Silver Sponsor)", logo: "/assets/sponsors/silver/glinttglobal.svg", cor: "#C4C4C4" },
    { nome: "Snapchat", logo: "/assets/speakers/snapchat.svg", cor: "#FFFC00" },
    { nome: "LTP (Silver Sponsor)", logo: "/assets/sponsors/silver/ltp.svg", cor: "#C4C4C4" },
    { nome: "Replai (Silver Sponsor)", logo: "/assets/sponsors/silver/replai.svg", cor: "#C4C4C4" },
    { nome: "Github", logo: "/assets/speakers/github.svg", cor: "#181717" },
    { nome: "Nutrium (Silver Sponsor)", logo: "/assets/sponsors/silver/nutrium.svg", cor: "#C4C4C4" },
    { nome: "AWS", logo: "/assets/speakers/aws.svg", cor: "#FF9900" },
    { nome: "Adidas (Bronze Sponsor)", logo: "/assets/sponsors/bronze/adidas.svg", cor: "#CE8946" }
  ];

  // Dados das Refeições
  const meals = [
    {
      titulo: "Pequeno-Almoço",
      icon: "fa-mug-hot",
      mealType: "pequeno-almoco",
      descricao: "Algo Simples, Saúdável e Móvel:",
      itens: [],
      opcoes: [
        { texto: "1 Pão com Manteiga / Doce", icon: "fa-bread-slice" },
        { texto: "Fatia de Queijo/Fiambre ou Ambos", icon: "fa-cheese" },
        { texto: "1 Queque/Fruta ", icon: "fa-cookie-bite" }
      ],
      bebidas: "1 Pacote de Sumo ou Leite p/pessoa"
    },
    {
      titulo: "Almoço / Jantar",
      icon: "fa-utensils",
      mealType: "almoco-jantar",
      descricao: "Comidas fáceis e económicas:",
      itens: [],
      opcoes: [
        { texto: "Massa à Bolonhesa", icon: "fa-bowl-food" },
        { texto: "Arroz de Pato", icon: "fa-drumstick-bite" },
        { texto: "Filetes de Peixe Dourados com Arroz", icon: "fa-fish" }
      ],
      bebidas: "Até 3 Copos de Sumo p/pessoa"
    },
    {
      titulo: "Coffee Break",
      icon: "fa-cookie-bite",
      mealType: "coffee-break",
      descricao: "Snacks rápidos nos intervalos:",
      itens: [],
      opcoes: [
        { texto: "Croissants / Mini Miniaturas", icon: "fa-stroopwafel" },
        { texto: "Queques Tradicionais", icon: "fa-cookie-bite" },
        { texto: "Fruta Fresca Variada", icon: "fa-apple-whole" }
      ],
      bebidas: "Água, Café e Chá Ilimitados"
    }
  ];

  // Dados dos Bilhetes
  const tickets = [
    {
      preco: "15€",
      nome: "Passe Geral (Estudantes)",
      cardType: "basico",
      beneficios: [
        { texto: "Acesso a 4 dias de evento", incluido: true },
        { texto: "Palestras & Workshops", incluido: true },
        { texto: "Feira de Empresas", incluido: true },
        { texto: "Sem alimentação", incluido: false },
        { texto: "Sem estadia", incluido: false }
      ]
    },
    {
      preco: "25€",
      nome: "Passe c/Alimentação (Estudantes)",
      cardType: "alimentacao",
      beneficios: [
        { texto: "Tudo do Passe Geral", incluido: true },
        { texto: "Almoço & Jantar", incluido: true },
        { texto: "Coffee Break", incluido: true },
        { texto: "Sem Pequeno Almoço", incluido: false },
        { texto: "Sem estadia", incluido: false }
      ]
    },
    {
      preco: "35€",
      nome: "Passe Completo (Estudantes)",
      cardType: "completo",
      beneficios: [
        { texto: "Tudo do Passe c/Alimentação", incluido: true },
        { texto: "Pequeno Almoço", incluido: true },
        { texto: "Estadia", incluido: true }
      ]
    },
    {
      preco: "TBD",
      nome: "Tertúlia VIP",
      cardType: "especial",
      incompleto: true,
      beneficios: [
        { texto: "Acesso ao dia da Tertúlia", incluido: true },
        { texto: "Benefícios em definição", incluido: true, incompleto: true }
      ]
    }
  ];

  // Dados Hierárquicos da Mesa (Direção)
  const boardStructure = {
    titulo: "Mesa da Organização",
    presidente: "David Gonçalves",
    vicePresidente: "Diogo Almeida",
    tesoureiro: "David Rodrigues",
    secretarios: [
      "David Silvestre", 
      "Afonso Bitoque"
    ],
    coordenadores: [
      "Afonso", 
      "Raquel", 
      "Rian"
    ]
  };

  // Dados dos Departamentos (Expansíveis e customizáveis)
  const departments = [
    {
      id: "logistica",
      nome: "Departamento de Logística",
      descCurta: "Espaços, alojamento e transportes.",
      icon: "fa-truck-ramp-box",
      diretor: "Barbara Pereira",
      membros: [
        "Beatriz Mateia; LEI", 
        "Guilherme Bacoco; LEI", 
        "Francisco Afonso; LEI", 
        "Lara; LESTI",
        "Ricardo Vicente; MEI"
      ]
    },
    {
      id: "marketing",
      nome: "Departamento de Marketing",
      descCurta: "Branding, redes e comunicação.",
      icon: "fa-bullhorn",
      diretor: "Raquel",
      membros: [
        "Jorge; mkt", 
        "Inês Campelo; Mkt"
      ]
    },
    {
      id: "comercial",
      nome: "Departamento Comercial",
      descCurta: "Sponsors, parcerias e orçamentos.",
      icon: "fa-wallet",
      diretor: "Mohammed Rohaim",
      membros: [
        "João Teixeira; LEI",
        "Duarte Cunha; LEI", 
        "José Tico; LEI",
        "Matilde; BioEng",
        "Tarsila; BioEng",
        "Marcio Andrade; LEI"
      ]
    },
    {
      id: "atividades",
      nome: "Departamento de Atividades",
      descCurta: "Social, networking e momentos de lazer.",
      icon: "fa-champagne-glasses",
      diretor: "Leonardo Cantachini",
      membros: [
        "Pedro Salgueiro; LEI", 
        "Gonçalo Santinho; LEI", 
        "Rodrigo Linhas; LEI", 
        "Afonso Francisco; LEI",
        "Simão Reis; LEI",
        "Rodrigo Silva; LEI",
        "Matheus Teixeira; LEI"
      ]
    },
    {
      id: "tecnologia",
      nome: "Departamento de Tecnologia",
      descCurta: "Website, inscrições e infraestrutura digital.",
      icon: "fa-laptop-code",
      diretor: "Francisco Melo",
      membros: [
        "Miguel Alito; LEI", 
        "Martim Neves; LEI", 
        "Guilherme; LEI",
        "Kakali; LEI",
        "Gabriel Vaz; LEI"
      ]
    }
  ];

  // Controlo dos Modais de Organização
  let activeModal = null; // 'mesa', 'logistica', 'marketing', etc.

  function openModal(id) {
    activeModal = id;
  }

  function closeModal() {
    activeModal = null;
  }

  // Lógica de Scroll do Carrossel de Bilhetes
  let ticketsGridElement;
  function scrollTickets(amount) {
    if (ticketsGridElement) {
      ticketsGridElement.scrollBy({ left: amount, behavior: 'smooth' });
    }
  }

  // Passos do Roadmap de Execução
  const roadmapSteps = [
    {
      data: "Abril 2026",
      texto: "Submissão Oficial da<br><strong>Pré-Candidatura</strong>",
      destaque: false
    },
    {
      data: "Jul-Ago 2026",
      texto: "Processo de Candidatura<br><span class=\"accent\">Definição do Vencedor</span>",
      destaque: false
    },
    {
      data: "Ago-Jan 2027",
      texto: "Angariação de Sponsors<br>Contacto com Entidades",
      destaque: false
    },
    {
      data: "Abril 2027",
      texto: "<span class=\"accent\">ENEI 2027</span><br><strong>FARO</strong>",
      destaque: true
    }
  ];

  // CONSTANTE DE PROGRESSO: Define qual o passo atual concluído/ativo (de 1 a 4)
  // Podes mudar este número manualmente (ex: 1, 2, 3 ou 4) e a barra encherá automaticamente!
  const currentRoadmapStep = 2;

  // Cálculo dinâmico do preenchimento da barra (0% a 100%)
  $: progressPercent = roadmapSteps.length > 1
    ? ((currentRoadmapStep - 1) / (roadmapSteps.length - 1)) * 100
    : 0;

  // Throttle State
  let lastTransitionTime = 0;
  const TRANSITION_THROTTLE_MS = 300;

  function canNavigate() {
    const now = Date.now();
    if (now - lastTransitionTime >= TRANSITION_THROTTLE_MS) {
      lastTransitionTime = now;
      return true;
    }
    return false;
  }

  function nextSlide() {
    if (currentSlide < totalSlides - 1) {
      currentSlide++;
    }
  }

  function prevSlide() {
    if (currentSlide > 0) {
      currentSlide--;
    }
  }

  function goToSlide(index) {
    if (index >= 0 && index < totalSlides) {
      currentSlide = index;
    }
  }

  // Fullscreen State
  let isFullscreen = false;

  function toggleFullscreen() {
    if (!document.fullscreenElement) {
      document.documentElement.requestFullscreen()
        .then(() => {
          isFullscreen = true;
        })
        .catch(err => {
          console.log(`Error attempting to enable fullscreen: ${err.message}`);
        });
    } else {
      document.exitFullscreen();
      isFullscreen = false;
    }
  }

  onMount(() => {
    // Teclado
    const handleKeyDown = (e) => {
      if (e.key === 'ArrowRight' || e.key === ' ') {
        e.preventDefault();
        if (canNavigate()) nextSlide();
      }
      if (e.key === 'ArrowLeft') {
        e.preventDefault();
        if (canNavigate()) prevSlide();
      }
    };

    // Touch
    let touchStartX = 0;
    let touchEndX = 0;
    let startedInCarousel = false;

    const handleTouchStart = (e) => {
      startedInCarousel = !!e.target.closest('.bilhetes-grid');
      touchStartX = e.changedTouches[0].screenX;
    };

    const handleTouchEnd = (e) => {
      if (startedInCarousel) return; // Permitir scroll interno do carrossel de bilhetes
      
      touchEndX = e.changedTouches[0].screenX;
      const diffX = touchStartX - touchEndX;
      if (Math.abs(diffX) > 50 && canNavigate()) {
        if (diffX > 0) {
          nextSlide();
        } else {
          prevSlide();
        }
      }
    };

    const handleFullscreenChange = () => {
      isFullscreen = !!document.fullscreenElement;
    };

    document.addEventListener('keydown', handleKeyDown);
    document.addEventListener('touchstart', handleTouchStart, { passive: true });
    document.addEventListener('touchend', handleTouchEnd, { passive: true });
    document.addEventListener('fullscreenchange', handleFullscreenChange);

    return () => {
      document.removeEventListener('keydown', handleKeyDown);
      document.removeEventListener('touchstart', handleTouchStart);
      document.removeEventListener('touchend', handleTouchEnd);
      document.removeEventListener('fullscreenchange', handleFullscreenChange);
    };
  });
</script>

<!-- Contador de slides -->
<div class="slide-counter">
  <span id="current-slide">{String(currentSlide + 1).padStart(2, '0')}</span>/{totalSlides}
</div>

<!-- Botão Fullscreen -->
<button 
  class="fullscreen-btn" 
  id="fullscreen-btn" 
  title="Fullscreen" 
  aria-label="Ativar modo ecrã inteiro"
  on:click={toggleFullscreen}
>
  <i class="fa-solid {isFullscreen ? 'fa-compress' : 'fa-expand'}"></i>
</button>

<div class="slides-wrapper">
  <!-- Slide 0: Capa -->
  <Slide title="Capa" index={0} {currentSlide} id="slide1">
    <div class="title-layout">
      <p class="subtitle">&lt;LOCAL_HOST: FARO_2027 /&gt;</p>
      <h1>ENEI 2027<br><span class="accent">Faro no Epicentro</span></h1>
      <p>Proposta Estratégica de Candidatura para o Maior Evento Nacional <br>de Estudantes de Engenharia Informática.</p>
    </div>
  </Slide>

  <!-- Slide 1: O Que é o ENEI? -->
  <Slide title="O Que é o ENEI?" index={1} {currentSlide} id="slide2">
    <h2 class="slide-title">O Que é o <span class="accent">ENEI</span>?</h2>
    <div class="content-area">
      <div class="two-column">
        <div class="tile">
          <div class="icon"><i class="fa-solid fa-users"></i></div>
          <h3>350+</h3>
          <p>Estudantes esperados de todo o país</p>
        </div>
        <div class="tile">
          <div class="icon"><i class="fa-solid fa-briefcase"></i></div>
          <h3>20+</h3>
          <p>Empresas de tecnologia nacionais e internacionais envolvidas.</p>
        </div>
        <div class="tile">
          <div class="icon"><i class="fa-solid fa-calendar"></i></div>
          <h3>4 Dias</h3>
          <p>De Quinta a Domingo, 4 dias repletos de conhecimento e networking.</p>
        </div>
        <div class="tile">
          <div class="icon"><i class="fa-solid fa-code"></i></div>
          <h3>20+</h3>
          <p>Masterclasses e sessões técnicas de alto nível.</p>
        </div>
      </div>
    </div>
  </Slide>

  <!-- Slide 2: Pilares Estratégicos -->
  <Slide title="Pilares Estratégicos" index={2} {currentSlide} id="slide3">
    <h2 class="slide-title">Pilares Estratégicos</h2>
    <div class="content-area">
      <div class="tiled-content">
        <div class="tile">
          <div class="icon"><i class="fa-solid fa-lightbulb"></i></div>
          <h3>Conhecimento</h3>
          <p>Transferência de saber crítico em IA, Cibersegurança e Engenharia de Software, entre outros...</p>
        </div>
        <div class="tile">
          <div class="icon"><i class="fa-solid fa-handshake"></i></div>
          <h3>Networking</h3>
          <p>Ponte direta entre o talento universitário e o ecossistema empresarial regional e internacional.</p>
        </div>
        <div class="tile">
          <div class="icon"><i class="fa-solid fa-compass"></i></div>
          <h3>Cidadania</h3>
          <p>Promoção de Faro como um Hub Tecnológico moderno e atrativo.</p>
        </div>
      </div>
    </div>
  </Slide>

  <!-- Slide 3: Empresas Parceiras -->
  <Slide title="Empresas Parceiras" index={3} {currentSlide} id="slide-empresas-parceiras">
    <h2 class="slide-title">Empresas de <span class="accent">Edições Anteriores</span></h2>
    <p class="empresas-subtitle">O ENEI tem um histórico de parcerias com marcas líderes globais e nacionais</p>
    <div class="content-area">
      <div class="past-sponsors-carousel">
        {#each pastSponsors as sponsor}
          <div class="past-sponsor-card">
            <div class="sponsor-top-bar" style="background-color: {sponsor.cor};"></div>
            <div class="sponsor-logo-container">
              <img src={sponsor.logo} alt={sponsor.nome} class="sponsor-logo" on:error={(e) => {
                e.target.style.display = 'none';
                e.target.nextElementSibling.style.display = 'block';
              }} />
              <div class="sponsor-logo-fallback" style="display: none; color: {sponsor.cor};">
                {sponsor.nome[0].toUpperCase()}{sponsor.nome.slice(1, 3).toLowerCase()}
              </div>
            </div>
            <span class="sponsor-name">{sponsor.nome}</span>
          </div>
        {/each}
      </div>
    </div>
  </Slide>

  <!-- Slide 4: Organização e Comissão -->
  <Slide title="Organização e Comissão" index={4} {currentSlide} id="slide-organizacao-comissao">
    <h2 class="slide-title">Organização & <span class="accent">Comissão</span></h2>
    <p class="comissao-subtitle" style="font-family: 'JetBrains Mono', monospace; font-size: 13px; color: var(--text-faint); margin-top: -30px; margin-bottom: 30px; text-transform: uppercase; letter-spacing: 1.5px; padding-left: 24px; text-align: left; width: 100%;">Liderado por David Gonçalves em parceria com o NEEI, NEEC, LESTI, BioEng, MKT e CC</p>
    
    <div class="content-area">
      <!-- Row Superior (2 colunas: 40% Info, 60% Mesa) -->
      <div class="comissao-row-top">
        <div class="comissao-info-card">
          <div class="comissao-card-tag">&lt;INFO /&gt;</div>
          <p>Esta comissão une múltiplos núcleos e cursos da UAlg sob uma visão comum: trazer o maior evento nacional de informática para o sul, promovendo a inovação regional.</p>
          <span class="comissao-click-tip">Clica num cartão ao lado ou abaixo para ver as equipas.</span>
        </div>

        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <div class="comissao-card-mesa" on:click={() => openModal('mesa')}>
          <div class="comissao-card-tag">&lt;BOARD /&gt;</div>
          <div class="comissao-card-inner">
            <div class="comissao-card-icon"><i class="fa-solid fa-users"></i></div>
            <div class="comissao-card-text">
              <h3>Mesa da Organização</h3>
              <p>Estrutura de liderança, coordenação executiva e secretaria geral da comissão.</p>
            </div>
          </div>
          <div class="comissao-card-footer">
            <span class="accent">Clique para Expandir a Hierarquia &rarr;</span>
          </div>
        </div>
      </div>

      <!-- Row Inferior (5 colunas: os 5 departamentos) -->
      <div class="comissao-row-bottom">
        {#each departments as dept}
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <!-- svelte-ignore a11y-no-static-element-interactions -->
          <div class="comissao-dept-card" on:click={() => openModal(dept.id)}>
            <div class="dept-header">
              <i class="fa-solid {dept.icon} accent"></i>
            </div>
            <div class="dept-body">
              <h4>{dept.nome.replace("Departamento de ", "")}</h4>
              <p>{dept.descCurta}</p>
            </div>
            <div class="dept-footer">
              <span>Ver Equipa &rarr;</span>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </Slide>

  <!-- Slide 5: Infraestruturas -->
  <Slide title="Infraestruturas" index={5} {currentSlide} customClass="slide-infra" id="slide5">
    <h2 class="slide-title">Infraestruturas <span class="accent">de Faro</span></h2>
    <p class="infra-subtitle">Planos estratégicos e alternativas de alojamento e sede para o ENEI 2027</p>
    <div class="content-area">
      <div class="infra-plans">
        <!-- Plano 1 -->
        <div class="plan-card plan-primary">
          <div class="plan-header">
            <span class="plan-number">01</span>
            <div class="plan-title-block">
              <span class="plan-badge">PRIMARY_PLAN</span>
              <h3>Plano Principal</h3>
            </div>
          </div>
          <div class="plan-content">
            <div class="plan-section">
              <i class="fa-solid fa-building"></i>
              <div>
                <strong>Sede do Evento</strong>
                <p>Conservatório Regional de Faro</p>
              </div>
            </div>
            <div class="plan-section">
              <i class="fa-solid fa-bed"></i>
              <div>
                <strong>Alojamento</strong>
                <p>Escola Secundária Tomás Cabreira</p>
              </div>
            </div>
            <div class="plan-images-row">
              <div class="plan-image-wrapper">
                <img src="/assets/images/conservatorio-algarve.jpg" alt="Conservatório Regional de Faro">
                <span class="image-label">Sede</span>
              </div>
              <div class="plan-image-wrapper">
                <img src="/assets/images/escola-nautical.jpg" alt="Escola Secundária Tomás Cabreira">
                <span class="image-label">Alojamento</span>
              </div>
            </div>
          </div>
        </div>

        <!-- Plano 2 -->
        <div class="plan-card plan-secondary">
          <div class="plan-header">
            <span class="plan-number">02</span>
            <div class="plan-title-block">
              <span class="plan-badge">BACKUP_A</span>
              <h3>Backup Plan A</h3>
            </div>
          </div>
          <div class="plan-content">
            <div class="plan-section">
              <i class="fa-solid fa-building"></i>
              <div>
                <strong>Sede do Evento</strong>
                <p>Universidade das Gambelas</p>
              </div>
            </div>
            <div class="plan-section">
              <i class="fa-solid fa-bed"></i>
              <div>
                <strong>Alojamento</strong>
                <p>Pavilhão Municipal da Penha</p>
              </div>
            </div>
            <div class="plan-images-row">
              <div class="plan-image-wrapper">
                <img src="/assets/images/ualg-1110.jpg" alt="Campus de Gambelas da UAlg">
                <span class="image-label">Sede</span>
              </div>
              <div class="plan-image-wrapper">
                <img src="/assets/images/pavilhao-penha.jpg" alt="Pavilhão Municipal da Penha">
                <span class="image-label">Alojamento</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </Slide>

  <!-- Slide 6: Plano de Execução -->
  <Slide title="Plano de Execução" index={6} {currentSlide} id="slide6">
    <h2 class="slide-title">Plano de Execução</h2>
    <div class="content-area">
      <div class="table-layout">
        <table>
          <thead>
            <tr>
              <th>Atividade</th>
              <th>Descrição</th>
              <th>Público Alvo</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><span class="accent">Feira de Empresas</span></td>
              <td>Feira de emprego com foco em recrutamento direto e contacto com empresas.</td>
              <td>Empresas & Todos os participantes</td>
            </tr>
            <tr>
              <td><span class="accent">Workshops</span></td>
              <td>Sessões práticas de 10 a 15 pessoas com profissionais da área.</td>
              <td>Estudantes</td>
            </tr>
            <tr>
              <td><span class="accent">Palestras</span></td>
              <td>Apresentações por profissionais de empresas nacionais e internacionais.</td>
              <td>Todos os participantes</td>
            </tr>
            <tr>
              <td><span class="accent">Tertúlias Académicas</span></td>
              <td>Conversas informais com professores da Universidade do Algarve.</td>
              <td>Estudantes</td>
            </tr>
            <tr>
              <td><span class="accent">Tertúlia Digital</span></td>
              <td>Encontro com influencers da área tech.</td>
              <td>Estudantes</td>
            </tr>
            <tr>
              <td><span class="accent">Passeio Matinal</span></td>
              <td>Sessão de relaxamento na praia ao nascer do sol.</td>
              <td>Todos os participantes</td>
            </tr>
            <tr>
              <td><span class="accent">CTF's</span></td>
              <td>Competições de Capture The Flag ao longo dos dias com prémios.</td>
              <td>Todos os participantes</td>
            </tr>
            <tr>
              <td><span class="accent">Atividades de Entrosamento</span></td>
              <td>Atividades de team building e interação entre os participantes.</td>
              <td>Estudantes</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </Slide>

  <!-- Slide 7: Calendário -->
  <Slide title="Calendário" index={7} {currentSlide} id="slide-calendario">
    <h2 class="slide-title">Calendário <span class="accent">ENEI 2027</span></h2>
    <p class="calendario-subtitle">4 dias de imersão tecnológica em Faro • 10:00 às 22:00</p>
    <div class="content-area">
      <div class="calendario-excel">
        <table class="tabela-horarios">
          <thead>
            <tr>
              <th class="col-hora">Hora</th>
              <th class="col-dia">Quinta (1 Abril)</th>
              <th class="col-dia">Sexta (2 Abril)</th>
              <th class="col-dia">Sábado (3 Abril)</th>
              <th class="col-dia">Domingo (4 Abril)</th>
            </tr>
          </thead>
          <tbody>
            <!-- Row 1: 10:00 -->
            <tr>
              <td class="hora-label">10:00</td>
              <td class="vazio" rowspan="4"></td>
              
              <!-- Sexta 10:00 -->
              <td class="celula-mista" rowspan="2">
                <div class="misto-wrapper">
                  <div class="palestras-stack">
                    <div class="inner-cell celula-palestra"><i class="fa-solid fa-microphone"></i> Palestra</div>
                    <div class="inner-cell celula-palestra"><i class="fa-solid fa-microphone"></i> Palestra</div>
                  </div>
                  <div class="workshops-grid">
                    <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #1</div>
                    <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #2</div>
                    <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #3</div>
                  </div>
                </div>
              </td>

              <!-- Sábado 10:00 -->
              <td class="celula-mista" rowspan="2">
                <div class="misto-wrapper">
                  <div class="palestras-stack">
                    <div class="inner-cell celula-palestra"><i class="fa-solid fa-microphone"></i> Palestra</div>
                    <div class="inner-cell celula-palestra"><i class="fa-solid fa-microphone"></i> Palestra</div>
                  </div>
                  <div class="workshops-grid">
                    <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #10</div>
                    <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #11</div>
                    <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #12</div>
                  </div>
                </div>
              </td>

              <!-- Domingo 10:00 -->
              <td class="celula-container" rowspan="2">
                <div class="single-wrapper span-2">
                  <div class="inner-cell celula-especial-laranja">
                    <i class="fa-solid fa-sun"></i> Nascer do Sol na Praia
                  </div>
                </div>
              </td>
            </tr>

            <!-- Row 2: 11:00 -->
            <tr>
              <td class="hora-label">11:00</td>
              <!-- Outras colunas cobertas por rowspan -->
            </tr>

            <!-- Row 3: 12:00 -->
            <tr>
              <td class="hora-label">12:00</td>
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-refeicao">
                    <i class="fa-solid fa-utensils"></i> Almoço
                  </div>
                </div>
              </td>
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-refeicao">
                    <i class="fa-solid fa-utensils"></i> Almoço
                  </div>
                </div>
              </td>
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-refeicao">
                    <i class="fa-solid fa-utensils"></i> Almoço
                  </div>
                </div>
              </td>
            </tr>

            <!-- Row 4: 14:00 -->
            <tr>
              <td class="hora-label">14:00</td>
              
              <!-- Sexta 14:00 -->
              <td class="celula-mista-longa" rowspan="4">
                <div class="misto-wrapper-longo">
                  <div class="palestras-stack-longo">
                    <div class="inner-cell celula-palestra"><i class="fa-solid fa-microphone"></i> Palestra</div>
                    <div class="inner-cell celula-palestra"><i class="fa-solid fa-microphone"></i> Palestra</div>
                    <div class="inner-cell celula-vazia"></div>
                    <div class="inner-cell celula-coffee"><i class="fa-solid fa-mug-hot"></i> Coffee Break</div>
                  </div>
                  <div class="workshops-stack-longo">
                    <div class="workshops-grid">
                      <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #4</div>
                      <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #5</div>
                      <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #6</div>
                    </div>
                    <div class="inner-cell celula-vazia"></div>
                  </div>
                </div>
              </td>

              <!-- Sábado 14:00 -->
              <td class="celula-mista-longa" rowspan="4">
                <div class="misto-wrapper-longo">
                  <div class="palestras-stack-longo">
                    <div class="inner-cell celula-palestra"><i class="fa-solid fa-microphone"></i> Palestra</div>
                    <div class="inner-cell celula-palestra"><i class="fa-solid fa-microphone"></i> Palestra</div>
                    <div class="inner-cell celula-vazia"></div>
                    <div class="inner-cell celula-coffee"><i class="fa-solid fa-mug-hot"></i> Coffee Break</div>
                  </div>
                  <div class="workshops-stack-longo">
                    <div class="workshops-grid">
                      <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #13</div>
                      <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #14</div>
                      <div class="inner-cell celula-workshop"><i class="fa-solid fa-laptop-code"></i> Workshop #15</div>
                    </div>
                    <div class="inner-cell celula-vazia"></div>
                  </div>
                </div>
              </td>

              <!-- Domingo 14:00 -->
              <td class="celula-container" rowspan="2">
                <div class="single-wrapper span-2">
                  <div class="inner-cell celula-tertulia">
                    <i class="fa-solid fa-comments"></i> Tertúlia
                  </div>
                </div>
              </td>
            </tr>

            <!-- Row 5: 15:00 -->
            <tr>
              <td class="hora-label">15:00</td>
              <td class="celula-container" rowspan="4">
                <div class="single-wrapper span-4">
                  <div class="inner-cell celula-checkin">
                    <i class="fa-solid fa-id-card"></i> Check-Ins
                  </div>
                </div>
              </td>
              <!-- Outras colunas cobertas por rowspan -->
            </tr>

            <!-- Row 6: 16:00 -->
            <tr>
              <td class="hora-label">16:00</td>
              <!-- Domingo 16:00 -->
              <td class="celula-container" rowspan="2">
                <div class="single-wrapper span-2">
                  <div class="inner-cell celula-encerramento">
                    <i class="fa-solid fa-trophy"></i> Sessão de Encerramento
                  </div>
                </div>
              </td>
              <!-- Outras colunas cobertas por rowspan -->
            </tr>

            <!-- Row 7: 16:30 -->
            <tr>
              <td class="hora-label">16:30</td>
              <!-- Outras colunas cobertas por rowspan -->
            </tr>

            <!-- Row 8: 17:00 -->
            <tr>
              <td class="hora-label">17:00</td>
              <!-- Quinta está coberta por rowspan do Check-In -->
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-palestra">
                    <i class="fa-solid fa-microphone"></i> Palestra
                  </div>
                </div>
              </td>
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-palestra">
                    <i class="fa-solid fa-microphone"></i> Palestra
                  </div>
                </div>
              </td>
              <td class="vazio" rowspan="4"></td>
            </tr>

            <!-- Row 9: 18:00 -->
            <tr>
              <td class="hora-label">18:00</td>
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-boas-vindas">
                    <i class="fa-solid fa-door-open"></i> Sessão de Boas Vindas
                  </div>
                </div>
              </td>
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-tertulia">
                    <i class="fa-solid fa-comments"></i> Tertulia
                  </div>
                </div>
              </td>
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-tertulia">
                    <i class="fa-solid fa-comments"></i> Tertulia
                  </div>
                </div>
              </td>
            </tr>

            <!-- Row 10: 20:00 -->
            <tr>
              <td class="hora-label">20:00</td>
              <td class="celula-container" rowspan="2">
                <div class="single-wrapper span-2">
                  <div class="inner-cell celula-arraial">
                    <i class="fa-solid fa-guitar"></i> Jantar/Arraial Académico
                  </div>
                </div>
              </td>
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-refeicao">
                    <i class="fa-solid fa-utensils"></i> Jantar
                  </div>
                </div>
              </td>
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-refeicao">
                    <i class="fa-solid fa-utensils"></i> Jantar
                  </div>
                </div>
              </td>
            </tr>

            <!-- Row 11: 22:00 -->
            <tr>
              <td class="hora-label">22:00</td>
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-atividades-noturnas">
                    <i class="fa-solid fa-moon"></i> Atividades Noturnas
                  </div>
                </div>
              </td>
              <td class="celula-container">
                <div class="single-wrapper span-1">
                  <div class="inner-cell celula-atividades-noturnas">
                    <i class="fa-solid fa-moon"></i> Atividades Noturnas
                  </div>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="legenda-calendario">
          <span class="legenda-item"><i class="fa-solid fa-laptop-code"></i> Workshops (Espaço Secundário)</span>
          <span class="legenda-item"><i class="fa-solid fa-microphone"></i> Palestras (Auditório Principal)</span>
          <span class="legenda-item"><i class="fa-solid fa-utensils"></i> Refeições (Almoço & Jantar)</span>
          <span class="legenda-item"><i class="fa-solid fa-mug-hot"></i> Coffee Break (Networking)</span>
        </div>
      </div>
    </div>
  </Slide>

  <!-- Slide 8: Alimentação -->
  <Slide title="Alimentação" index={8} {currentSlide} id="slide-alimentacao">
    <h2 class="slide-title">Plano de <span class="accent">Alimentação</span></h2>
    <p class="alimentacao-subtitle">Refeições nutritivas e económicas para todos os participantes</p>
    <div class="content-area">
      <div class="refeicoes-grid">
        {#each meals as meal}
          <MealCard
            titulo={meal.titulo}
            icon={meal.icon}
            mealType={meal.mealType}
            descricao={meal.descricao}
            opcoes={meal.opcoes}
            bebidas={meal.bebidas}
          />
        {/each}
      </div>
      <p class="alimentacao-nota"><i class="fa-solid fa-circle-info"></i> Opções vegetarianas disponíveis mediante pedido prévio</p>
    </div>
  </Slide>

  <!-- Slide 9: Planos de Bilhetes -->
  <Slide title="Planos de Bilhetes" index={9} {currentSlide} id="slide-bilhetes">
    <h2 class="slide-title">Planos de <span class="accent">Bilhetes</span></h2>
    <p class="bilhetes-subtitle">Escolhe o plano que melhor se adapta às tuas necessidades</p>
    <div class="content-area">
      <div class="carousel-wrapper">
        <button class="carousel-arrow prev" on:click={() => scrollTickets(-330)} aria-label="Anterior">
          <i class="fa-solid fa-chevron-left"></i>
        </button>
        <div class="bilhetes-grid" bind:this={ticketsGridElement}>
          {#each tickets as ticket}
            <TicketCard
              preco={ticket.preco}
              nome={ticket.nome}
              publico={ticket.publico}
              cardType={ticket.cardType}
              incompleto={ticket.incompleto}
              beneficios={ticket.beneficios}
            />
          {/each}
        </div>
        <button class="carousel-arrow next" on:click={() => scrollTickets(330)} aria-label="Seguinte">
          <i class="fa-solid fa-chevron-right"></i>
        </button>
      </div>
      <div class="carousel-indicator">
        <span class="indicator-tip"><i class="fa-solid fa-left-right"></i> Arraste para ver todos os planos</span>
      </div>
    </div>
  </Slide>

  <!-- Slide 10: Roadmap -->
  <Slide title="Roadmap" index={10} {currentSlide} id="slide10">
    <h2 class="slide-title">Roadmap de <span class="accent">Execução</span></h2>
    <div class="content-area">
      <div class="timeline-layout">
        <div class="timeline-line"></div>
        <div class="timeline-progress" style="width: {progressPercent}%"></div>
        
        {#each roadmapSteps as step, i}
          <div class="timeline-item {step.destaque ? 'destaque' : ''} {i < currentRoadmapStep ? 'active' : ''}">
            <h3>{step.data}</h3>
            <div class="timeline-dot {i < currentRoadmapStep ? 'active' : ''}"></div>
            <p>{@html step.texto}</p>
          </div>
        {/each}
      </div>
    </div>
  </Slide>

  <!-- Slide 11: Contacto -->
  <Slide title="Contacto" index={11} {currentSlide} customClass="slide-final" id="slide11">
    <div class="title-layout">
      <h2 class="accent">Vamos Construir o Futuro?</h2>
      <p>David Gonçalves | Presidente da Comissão Organizadora do ENEI Faro 2027</p>
      <div class="contact-box contact-multi">
        <div class="contact-item">
          <i class="fa-solid fa-phone"></i>
          <span>919 105 170</span>
        </div>
        <div class="contact-item">
          <i class="fa-solid fa-envelope"></i>
          <span>davidpgoncalves03@gmail.com</span>
        </div>
        <div class="contact-item">
          <i class="fa-solid fa-building-columns"></i>
          <span>coenei@aaualg.pt</span>
        </div>
      </div>
    </div>
  </Slide>
</div>

<!-- Modal de Hierarquia de Equipas (Mesa & Departamentos) -->
{#if activeModal}
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div class="modal-overlay" on:click={closeModal}>
    <div class="modal-container" on:click|stopPropagation>
      <button class="modal-close" on:click={closeModal}>
        <i class="fa-solid fa-xmark"></i>
      </button>

      {#if activeModal === 'mesa'}
        <!-- Árvore Hierárquica Completa da Mesa -->
        <div class="tree-wrapper">
          <div class="tree-header">
            <span class="accent">&lt;BOARD_TREE /&gt;</span>
            <h2>{boardStructure.titulo}</h2>
          </div>

          <div class="tree-node root-node">
            <div class="node-tag">PRESIDENTE</div>
            <div class="node-name">{boardStructure.presidente}</div>
          </div>

          <div class="tree-connector-vertical"></div>

          <div class="tree-row-two">
            <div class="tree-node parent-node">
              <div class="node-tag">VICE-PRESIDENTE</div>
              <div class="node-name">{boardStructure.vicePresidente}</div>
            </div>
            
            <div class="tree-node parent-node">
              <div class="node-tag">TESOUREIRO</div>
              <div class="node-name">{boardStructure.tesoureiro}</div>
            </div>
          </div>

          <div class="tree-connector-vertical"></div>

          <div class="tree-row-three">
            <div class="tree-col">
              <div class="tree-section-title">SECRETARIA</div>
              {#each boardStructure.secretarios as sec}
                <div class="tree-node child-node">
                  <div class="node-name">{sec}</div>
                </div>
              {/each}
            </div>

            <div class="tree-col">
              <div class="tree-section-title">COORDENAÇÃO CURSOS</div>
              <div class="coordenadores-group">
                {#each boardStructure.coordenadores as coord}
                  <div class="tree-node child-node">
                    <div class="node-name">{coord}</div>
                  </div>
                {/each}
              </div>
            </div>
          </div>
        </div>
      {:else}
        <!-- Árvore Simplificada de Departamento (Diretor -> Membros) -->
        {@const dept = departments.find(d => d.id === activeModal)}
        {#if dept}
          <div class="tree-wrapper dept-tree">
            <div class="tree-header">
              <span class="accent">&lt;DEPT_TREE /&gt;</span>
              <h2>{dept.nome}</h2>
              <p>{dept.descCurta}</p>
            </div>

            <div class="tree-node root-node">
              <div class="node-tag">COORDENADOR DE DEPARTAMENTO</div>
              <div class="node-name">{dept.diretor}</div>
            </div>

            <div class="tree-connector-vertical"></div>

            <div class="tree-section-title">EQUIPA DE MEMBROS</div>
            <div class="dept-members-grid">
              {#each dept.membros as membro}
                {@const parts = membro.split(';')}
                <div class="tree-node child-node">
                  <div class="node-name">
                    {parts[0].trim()}
                    {#if parts[1]}
                      <span class="member-course">({parts[1].trim()})</span>
                    {/if}
                  </div>
                </div>
              {:else}
                <p class="no-members">Nenhum membro adicionado a esta equipa.</p>
              {/each}
            </div>
          </div>
        {/if}
      {/if}
    </div>
  </div>
{/if}

<!-- Barra de Título -->
<div class="slide-title-bar">
  <span class="slide-title-display" id="slide-title">{slideTitles[currentSlide]}</span>
</div>

<!-- Controles de Navegação -->
<div class="slide-controls">
  <button 
    class="nav-arrow" 
    id="prev-btn" 
    aria-label="Slide anterior"
    on:click={() => { if (canNavigate()) prevSlide(); }}
  >
    <i class="fa-solid fa-chevron-left"></i>
  </button>
  
  <div class="slide-dots" id="slide-dots">
    {#each Array(totalSlides) as _, i}
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <!-- svelte-ignore a11y-no-static-element-interactions -->
      <div 
        class="dot {i === currentSlide ? 'active' : ''}" 
        on:click={() => goToSlide(i)}
      ></div>
    {/each}
  </div>
  
  <button 
    class="nav-arrow" 
    id="next-btn" 
    aria-label="Slide seguinte"
    on:click={() => { if (canNavigate()) nextSlide(); }}>
    <i class="fa-solid fa-chevron-right"></i>
  </button>
</div>
