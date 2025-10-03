<script setup>
import projects from "../data/projects.json";
</script>
<template>
  <div id="projects" class="container-fluid text-center p-5">
    <div class="row">
      <div class="col-12">
        <h2>PROJECTS</h2>
        <p>Things I've built so far</p>
      </div>
    </div>
    <div class="row card-row">
      <template v-for="project in projects" :key="project.id">
        <div class="col-lg-3 col-md-6 g-4 d-flex">
          <div class="card w-100">
            <img :src="project.image" class="card-img-top" alt="..." />
            <div class="card-body d-flex flex-column">
              <h5 class="card-title">{{ project.title }}</h5>
              <p class="card-text">
                {{ project.description }}
              </p>
            </div>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>
