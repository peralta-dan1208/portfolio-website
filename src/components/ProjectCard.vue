<template>
	<component
	  :is="project.link || project.title === 'Portfolio Website(Capstone)' ? 'a' : 'div'"
	  :href="project.link || '#'"
	  :target="project.link ? '_blank' : undefined"
	  :rel="project.link ? 'noopener noreferrer' : undefined"
	  @click="project.title === 'Portfolio Website(Capstone)' ? scrollToTop($event) : null"
	  class="card col-md-3 m-2 rounded shadow text-decoration-none text-dark card-hover"
	  id="carouselItem"
	>
	<div class="h-100 d-flex flex-column">
		<img :src="project.image" class="card-img-top card-img mt-3" alt="...">
		<div class="card-body d-flex flex-column flex-grow-1">
			<h4 class="card-title">{{project.title}}</h4>
			<p class="card-text flex-grow-1">
				{{project.description}}
			</p>
			<small v-if="!project.link" class="text-muted mt-auto">
	       	WIP
	      	</small>
		</div>
	</div>
	</component>
</template>

<script setup>
import { defineProps } from 'vue';

defineProps({
	project: Object
})

const scrollToTop = (event) => {
  event.preventDefault();
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  });
};
</script>