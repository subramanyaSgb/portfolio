# 3D Industrial Factory Portfolio — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Transform the existing 2D portfolio into a fully immersive scroll-driven 3D factory tour using Three.js and GSAP.

**Architecture:** Single `index.html` file. Three.js renders a procedural 3D factory scene on a full-screen canvas. GSAP ScrollTrigger drives camera movement along a predefined path through the factory. HTML content overlays appear at each station, using CSS `position: fixed` sections that fade in/out based on scroll position. WebGL fallback keeps the current 2D site for unsupported browsers.

**Tech Stack:** Three.js r160 (CDN), GSAP 3.12 + ScrollTrigger (CDN), vanilla JS, CSS

---

### Task 1: Scaffold — Three.js Canvas + GSAP ScrollTrigger Boilerplate

**Files:**
- Modify: `index.html`

**Step 1: Add CDN scripts and base structure**

Replace the entire `index.html` with a new structure that:
- Loads Three.js, GSAP, and ScrollTrigger from CDN
- Creates a `<canvas id="factory">` fixed behind all content
- Wraps existing HTML content sections in a `<div class="scroll-container">` with enough height for scroll (700vh)
- Adds WebGL detection — if no WebGL, shows the original 2D layout
- Initializes Three.js scene, camera (PerspectiveCamera), renderer (WebGLRenderer with antialiasing)
- Sets up a render loop with `requestAnimationFrame`
- Registers GSAP ScrollTrigger plugin

**Step 2: Verify base renders**

Open `index.html` in browser. You should see:
- A black canvas covering the viewport
- Page scrolls but canvas stays fixed
- No console errors
- Three.js scene renders (just black for now)

**Step 3: Commit**

```bash
git add index.html
git commit -m "feat: scaffold Three.js canvas + GSAP ScrollTrigger base"
```

---

### Task 2: Factory Floor, Walls, and Ceiling

**Files:**
- Modify: `index.html` (inside the Three.js scene setup)

**Step 1: Build the factory shell**

Add procedural geometry for:
- **Floor**: Large PlaneGeometry (200x800), dark grey MeshStandardMaterial with roughness, rotated -90deg on X. Grid lines via a second wireframe plane on top.
- **Walls**: Two BoxGeometry walls (height 30, length 800) on each side, dark metallic material with emissive edges.
- **Ceiling**: PlaneGeometry above, darker material, with rectangular cutouts simulated by darker patches.
- **Pillars**: Repeating CylinderGeometry columns every 80 units along each wall (steel grey material).

**Step 2: Add industrial lighting**

- 1 warm AmbientLight (0x1a1a2e, intensity 0.3)
- 2-3 orange PointLights hanging from ceiling simulating industrial lamps (0xff6b35, intensity 1, distance 100)
- 1 green accent SpotLight (0x10b981) that follows camera loosely
- Add fog: `scene.fog = new THREE.FogExp2(0x0a0a0b, 0.008)`

**Step 3: Verify**

Open browser. You should see a dark industrial corridor stretching into the distance with warm orange lighting pools and fog. Scrolling does nothing yet (camera static).

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add factory floor, walls, ceiling, and lighting"
```

---

### Task 3: Camera Path + Scroll-Driven Movement

**Files:**
- Modify: `index.html`

**Step 1: Define camera path waypoints**

Create an array of camera positions and lookAt targets for each station:
```
Station 0 (Hero):     pos(0, 8, 80)    → look(0, 5, 0)
Station 1 (About):    pos(0, 6, 40)    → look(5, 5, 30)
Station 2 (Skills):   pos(0, 7, 0)     → look(-5, 5, -10)
Station 3 (Experience): pos(0, 6, -50)  → look(0, 4, -60)
Station 4 (Clients):  pos(0, 8, -100)  → look(0, 6, -110)
Station 5 (Projects):  pos(0, 7, -160) → look(5, 5, -170)
Station 6 (Contact):  pos(0, 6, -220)  → look(0, 5, -230)
```

**Step 2: Wire GSAP ScrollTrigger to camera**

Use GSAP timeline with ScrollTrigger:
- `trigger: ".scroll-container"`
- `start: "top top"`, `end: "bottom bottom"`
- `scrub: 1.5` (smooth scroll-to-animation mapping)
- Animate `camera.position` and a `lookAtTarget` vector through each waypoint
- Use `onUpdate` to call `camera.lookAt(lookAtTarget)`

**Step 3: Verify**

Scroll the page. Camera should glide smoothly through the factory corridor, moving from entrance to the far end. Fog creates depth.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: scroll-driven camera path through factory"
```

---

### Task 4: Station 0 — Factory Entrance (Hero)

**Files:**
- Modify: `index.html`

**Step 1: Build entrance geometry**

- Large gate frame: Two vertical BoxGeometry pillars + horizontal beam across top
- "SGB" text as a glowing sign above the gate (use Three.js TextGeometry or a simple plane with canvas-rendered text)
- Spark particles: BufferGeometry with ~200 points, PointsMaterial (orange/yellow), animated positions falling like welding sparks
- Floor markings: yellow hazard stripes near entrance (thin box geometries)

**Step 2: Hero HTML overlay**

Position the hero section content (name, title, stats) as a fixed overlay that fades in when scroll is 0-15% and fades out after. Use GSAP ScrollTrigger to animate opacity.

**Step 3: Verify**

On load, see factory gate with "SGB" sign, sparks falling, hero text overlay visible. As you scroll, hero fades and camera moves forward.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: factory entrance with gate, sparks, and hero overlay"
```

---

### Task 5: Station 1 — Control Room (About)

**Files:**
- Modify: `index.html`

**Step 1: Build control room geometry**

At z=30 area:
- Desk/console: BoxGeometry table with a slightly emissive top surface
- Monitor screens: 3-4 PlaneGeometry "screens" angled on the desk, glowing green/blue edges (EdgesGeometry with green LineBasicMaterial)
- Control panels: Small box geometries on the wall with colored "LED" dots (tiny sphere meshes with emissive materials)

**Step 2: About HTML overlay**

The existing About section content appears as a fixed overlay, fading in at scroll 15-30%. Info cards styled as holographic panels (glassmorphism CSS with green glow border).

**Step 3: Commit**

```bash
git add index.html
git commit -m "feat: control room station with about overlay"
```

---

### Task 6: Station 2 — Assembly Floor (Skills)

**Files:**
- Modify: `index.html`

**Step 1: Build assembly floor geometry**

At z=0 to z=-30 area:
- Machinery silhouettes: Groups of box/cylinder geometries suggesting industrial machines
- Overhead crane: A beam across the ceiling with a hanging hook (cylinder + small box)
- Conveyor belt section: A flat animated plane with subtle UV scrolling texture effect

**Step 2: Skills HTML overlay**

Skills section fades in at scroll 30-45%. Skill tags float in with staggered animation.

**Step 3: Commit**

```bash
git add index.html
git commit -m "feat: assembly floor station with skills overlay"
```

---

### Task 7: Station 3 — Production Line (Experience)

**Files:**
- Modify: `index.html`

**Step 1: Build production line geometry**

At z=-50 to z=-80 area:
- Conveyor belt: Long PlaneGeometry with animated UV offset (moving texture effect)
- Rollers: Repeating CylinderGeometry under the belt
- Station markers: Glowing vertical lines or small podiums at each role position along the belt

**Step 2: Experience HTML overlay**

Timeline section fades in at scroll 45-60%. Timeline items animate in one by one with GSAP stagger.

**Step 3: Commit**

```bash
git add index.html
git commit -m "feat: production line station with experience overlay"
```

---

### Task 8: Station 4 — Trophy Wall (Clients) + Station 5 — Project Bays (Projects)

**Files:**
- Modify: `index.html`

**Step 1: Build trophy wall**

At z=-100 to z=-120:
- A flat wall section with 6 rectangular "plaques" (BoxGeometry, metallic material)
- Each plaque has subtle glow edge matching the client colors
- Clients overlay fades in at scroll 60-70%

**Step 2: Build project bays**

At z=-140 to z=-200:
- Individual "work bays" — open-front box rooms along the walls
- Each bay has a monitor screen (PlaneGeometry with emissive glow)
- Projects overlay with filter tabs fades in at scroll 70-85%
- Project filter functionality preserved from original

**Step 3: Commit**

```bash
git add index.html
git commit -m "feat: trophy wall and project bays stations"
```

---

### Task 9: Station 6 — Office (Contact) + Footer

**Files:**
- Modify: `index.html`

**Step 1: Build office geometry**

At z=-220 to z=-250:
- Warmer lighting (slightly brighter ambient)
- Desk with monitor
- A door frame at the far end (suggesting "exit" / end of tour)

**Step 2: Contact HTML overlay**

Contact section fades in at scroll 85-100%. Contact buttons styled as terminal/monitor buttons.

**Step 3: Commit**

```bash
git add index.html
git commit -m "feat: office station with contact overlay and footer"
```

---

### Task 10: Ambient Effects + Polish

**Files:**
- Modify: `index.html`

**Step 1: Add ambient particles**

- Dust/steam particles throughout factory (BufferGeometry, ~500 points, very small, slow drift)
- Subtle glow halos around lights (sprite with radial gradient texture)

**Step 2: Add post-processing feel via CSS**

- Scanline overlay (already exists in CSS — keep it)
- Subtle vignette via CSS radial gradient on the canvas container
- Noise overlay (already exists — keep it)

**Step 3: Add scroll progress indicator**

- Thin green progress bar at the top of the viewport showing scroll %

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: ambient particles, polish, and scroll indicator"
```

---

### Task 11: Mobile Optimization + WebGL Fallback

**Files:**
- Modify: `index.html`

**Step 1: Mobile detection and simplification**

- Detect mobile via `window.innerWidth < 768`
- On mobile: reduce particle count by 75%, remove some station geometry, simplify lighting (fewer point lights)
- Reduce renderer pixel ratio on mobile: `renderer.setPixelRatio(Math.min(window.devicePixelRatio, 1.5))`

**Step 2: WebGL fallback**

- Check `WebGLRenderingContext` support
- If no WebGL: hide canvas, show a simplified version of the 2D layout (the original CSS design)

**Step 3: Handle resize**

- `window.addEventListener('resize', ...)` — update camera aspect, renderer size

**Step 4: Verify on mobile viewport**

Use browser DevTools mobile emulation. Should run smoothly at 60fps.

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: mobile optimization and WebGL fallback"
```

---

### Task 12: Final Push to GitHub Pages

**Files:**
- Modify: `index.html` (final tweaks if any)

**Step 1: Final review**

- Scroll through entire site, verify all 7 stations render and overlay correctly
- Check console for errors
- Verify mobile viewport

**Step 2: Push**

```bash
git push origin main
```

GitHub Pages will auto-deploy the updated site.
