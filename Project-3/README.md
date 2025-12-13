# Project 3 – WebGL Procedural Geometry  
CS 351 – Computer Architecture

Zainab Ghafoor

This project explores procedural geometry in WebGL by generating shapes entirely inside the vertex shader using `gl_VertexID`. Each version builds on the previous one.

---

## Table of Contents 

1. **[Wireframe Triangle](triangle.html)**
2. **[10-Sided Filled Polygon](disk.html)**
3. **[Five-Pointed Colored Star](start.html)**
4. **[Five-Pointed Rotating Colored Star](start-commented.html)**

---

## Image Previews 

[![Triangle](triangle.png)](triangle.html)  
[![Polygon](disk.png)](disk.html)  
[![Colored Star](start.png)](start.html)  
[![Rotating Colored Star](start-commented.gif)](start-commented.html)

---

## Descriptions

### 1. Wireframe Triangle (`triangle.html`)
Procedurally generates an equilateral triangle in the vertex shader using gl_VertexID and trigonometric functions. The triangle is rendered as a wireframe using gl.LINE_LOOP, with no vertex buffers or external geometry.

### 2. 10-Sided Filled Polygon (`disk.html`)
Generates a filled 10-sided polygon procedurally using a triangle fan, with vertex positions computed in the vertex shader from gl_VertexID.

### 3. Five-Pointed Colored Star (`start.html`)
Procedurally generates a five-pointed star by alternating vertex radii using gl_VertexID. Colors are computed per vertex in the vertex shader using trigonometric functions, creating a smooth rainbow effect.

### 4. Five-Pointed Rotating Colored Star (`start-commented.html`)
Procedurally generates a five-pointed star using gl_VertexID and a triangle fan, with alternating inner and outer radii. A time uniform is used to continuously rotate the star, and per-vertex colors are computed in the vertex shader using trigonometric functions, producing a smooth animated rainbow effect.
