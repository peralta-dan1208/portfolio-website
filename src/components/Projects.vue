<template>
  <div id="projects" class="container mb-3">
    <h1 class="text-center pt-5 mt-5">My Projects</h1>

    <div class="row p-5 justify-content-center">
      <div id="carouselExampleControls" class="carousel slide" data-ride="carousel" data-interval="false">
        <div class="carousel-inner">
          <div class="carousel-item" v-for="(group, index) in chunkedProjects" :key="index" :class="{ active: index === 0 }" >
            <div class="row justify-content-center">
              <div class="col-md-3 m-2 rounded shadow" id="carouselItem" v-for="project in group" :key="project.id" >
                <img :src="project.image" class="img-fluid mx-auto d-block p-3" />
                <h3>{{ project.title }}</h3>
                <p>{{ project.description }}</p>
              </div>
            </div>
          </div>

        </div>

        <!-- Controls -->
        <a class="carousel-control-prev" href="#carouselExampleControls" role="button" data-slide="prev" >
          <span class="carousel-control-prev-icon"></span>
        </a>

        <a class="carousel-control-next" href="#carouselExampleControls" role="button" data-slide="next" >
          <span class="carousel-control-next-icon"></span>
        </a>

      </div>
    </div>
  </div>
</template>

<script setup>
	// import the 'computed' method to define reactive derived state
	import { computed } from 'vue';
	import ProjectCard from './ProjectCard.vue';
	import projects from '../data/projects.json';

	// number of projects displayed per row
	const chunkSize = 3;

	// the main purpose of the function is to create smaller arrays inside of the projects that contains 3(chunkSize) projects each
	const chunkedProjects = computed(() => {
		const chunks = [];

		for(let i = 0; i < projects.length; i+= chunkSize) {
			chunks.push(projects.slice(i,i+chunkSize));
		}

		return chunks;
	})
</script>