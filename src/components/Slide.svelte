<script>
  export let title = "";
  export let index = 0;
  export let currentSlide = 0;
  export let customClass = "";
  export let id = "";

  $: isActive = index === currentSlide;
</script>

<section class="slide {isActive ? 'active' : ''} {customClass}" data-title={title} data-index={index}>
  <div class="slide-container" {id}>
    <slot />
  </div>
</section>
