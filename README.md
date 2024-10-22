# CUDA Path Tracer

* [Intro](#Introduction)
* [Features](#Features)
* * [BSDFs](#BSDFs)
* * [Integrators](#Integrators)
* * [Mesh and Texture Loading](#Mesh-Loading)
* * [Denoiser](#OIDN)
* [Perf Analysis](#Perf-Analysis)
* [Bloopers](#Bloopers)
* [Credits](#Credits)
-----

##### Example Render:
<p align="center">
  <img src="img/HeroRender.png" width="1000" />
</p>

###### 2350 x 1000 | CUDA Path Tracer with Intel OIDN Denoiser | BVH | 500 SPP | All assets modelled in Maya

<p align="center">
  <img src="img/Breakdown.png" width="500" />
</p>

---

### University of Pennsylvania, CIS 565: GPU Programming and Architecture, Project 3
* Logan Cho
  * [LinkedIn](https://www.linkedin.com/in/logan-cho/)
  * [Art / Coding Portfolio!](https://www.logancho.com/)
* Run on: Windows 11, 13th Gen Intel(R) Core(TM) i7-13700H, 2.40 GHz, RTX 4060 Laptop GPU
-----

## Introduction

This project is a CUDA Path Tracer developed on top of the base code provided by the University of Pennsylvania, CIS 565: GPU Programming and Architecture. Features implemented include: 
* Parallelized Naive Lighting Integrator
* Parallelized Full Lighting Integrator with MIS
* Performance Optimizations
  * Sorting Path Segments by material type to reduce divergence and increase warp occupancy
  * Stream Compaction of Path Segments to free up cores from rays that have already terminated
* Arbitrary .glTF mesh support with materials and textures (albedo and normals)
* BSDFs (Diffuse, Specular, Microfacet, etc.)
* Intel OIDN Integration (Denoiser)

## Features

### BSDFs

* #### Diffuse

<p align="center">
  <img src="img/cornell/4.png" width="300" />
</p>

* #### Specular
<p align="center">
  
  | <img width="300px" src="img/cornell/naive.png"> | <img width="300px" src="img/cornell/1.png"> | <img width="300px" src="img/sonic/11.png"> |
  |:--:|:--:|:--:|
  
</p>

* #### Microfacet

<p align="center">
  
  | <img width="300px" src="img/cornell/3.png"> | <img width="300px" src="img/sonic/1.png"> |
  |:--:|:--:|
  
</p>

* #### Ceramic / Plastic

<p align="center">
  
  | <img width="300px" src="img/cornell/2.png"> | <img width="300px" src="img/sonic/9.png"> |
  |:--:|:--:|
  
</p>

---

### Integrators

I currently support 3 Lighting Integrator models: Naive, Full Lighting with MIS and Direct Lighting. The following renders are after only a few samples (10.) You can see that the direct lighting and full lighting integrator models are far less noisy than the naive integrator. 

| <img width="300px" src="img/integrators/naive.png"> | <img width="300px" src="img/integrators/full.png"> | <img width="300px" src="img/integrators/direct.png"> |
|:--:|:--:|:--:|
| *Naive Integrator @ 10spp* | *Full Lighting Integrator with MIS @ 10spp* | *Direct Lighting Integrator @ 10spp* |

Currently, there is a bug which doesn't allow my Full Lighting model to correctly render with more than 1 light source. TBC.

---

### Mesh Loading

This path tracer supports .glTF 3D scene loading and rendering. This was done through wrapping the [tinyGLTF library](https://github.com/syoyo/tinygltf). Here are the supported capabilities:
* Triangular Mesh Loading
* Material Loading
* Albedo Texture Loading and Sampling
* Pbject Space Normal Map Loading and Sampling

There are a few restrictions however:
* The mesh must be triangulated. Only triangles are supported currently.
* Materials must be mapped manually in your Path Tracer .json file. That is, if your glTF file has 4 unique materials, then you must define 4 materials in your .json file accordingly to allow for the 4 materials to appear in the render.

---

### OIDN

This path tracer supports a machine learning denoiser to modify the final render output. The user can use the UI to configure the strength of denoising they want to apply.

[Intel OIDN](https://www.openimagedenoise.org/)

## Perf Analysis

### BVH

### Sort by Material

### Stream Compaction

## Bloopers

## Credits

