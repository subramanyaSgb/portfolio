# 3D Interactive Industrial Factory Portfolio

## Concept
Scroll-driven tour through a stylized 3D steel factory. Each portfolio section is a factory station.

## Stations (by scroll position)
1. **Factory Entrance (Hero)** - Camera approaches glowing factory gate. Name/title as industrial signage. Sparks and particles.
2. **Control Room (About)** - Bio on holographic panels. Info cards float in 3D.
3. **Assembly Floor (Skills)** - Skill groups on industrial panels/screens mounted on machinery.
4. **Production Line (Experience)** - Conveyor belt timeline. Each role is a station along the belt.
5. **Trophy Wall (Clients)** - Client logos on steel plaques.
6. **Project Bays (Projects)** - Project cards as workstation monitors in factory bays. Filters still work.
7. **Office (Contact)** - Contact buttons as interactive terminals.

## Technical Stack
- Three.js (CDN) - 3D rendering
- GSAP + ScrollTrigger (CDN) - Scroll-based camera animation
- Single index.html - GitHub Pages compatible
- Low-poly procedural geometry - No external 3D model files

## 3D Elements (all procedural)
- Factory walls, floor, ceiling (box geometry)
- Conveyor belts (animated geometry)
- Glowing pipes and machinery silhouettes
- Particle effects (sparks, steam, dust)
- Holographic text panels (glass-like planes with content)
- Dynamic lighting (warm orange industrial + green accent)

## Performance
- WebGL fallback to 2D site
- Mobile: reduced particles, simplified geometry
- Target: 60fps on mid-range devices
- ~150-200KB total (no model files)

## Preserved
- All existing content
- Color scheme (dark bg, green accent)
- Mobile responsiveness
