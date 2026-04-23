<template>
  <div id="projects" class="container mb-5">
    <h1 class="text-center pt-5 mt-5">My Projects</h1>
    <div class="row p-5 justify-content-center">
      <div id="carouselExampleControls" class="carousel slide" data-ride="carousel" data-interval="false">
        <div class="carousel-inner">
          <div class="carousel-item" v-for="(group,index) in chunkedProjects" :key="index" :class="{ active: index === 0 }">
            <div class="row justify-content-center">
              <ProjectCard v-for="project in group" :key="project.id" :project="project"/>
            </div>
          </div>
        </div>
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
	import { computed } from 'vue';
	import ProjectCard from './ProjectCard.vue';
	import projects from '../data/projects.json';

	const chunkSize = 3;

	const chunkedProjects = computed(() => {
		const chunks = [];

		for(let i = 0; i < projects.length; i+= chunkSize) {
			chunks.push(projects.slice(i,i+chunkSize));
		}

		return chunks;
	})
</script>