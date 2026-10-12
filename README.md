<h1>3D Sand Disintegration &amp; Reconstruction – Three.js, WebGL &amp; GLSL</h1>
<p><strong>Turn a 3D model into thousands of animated sand particles — and reconstruct its original shape in real time.</strong></p>
<p>I developed this project as an experiment in procedural 3D graphics, GPU-powered particle animation, and interactive visual effects using <strong>JavaScript, Three.js, WebGL, and GLSL shaders</strong>.</p>
<p>My goal was to build something more versatile than a simple animated logo.</p>
<p>The underlying system works with triangulated 3D geometry. It samples points across a model's surface, assigns each particle a displacement vector, and uses a vertex shader to animate the transition between a solid object and a scattered cloud of particles.</p>
<p>For this demonstration, I used the <strong>Design Cart 3D logo</strong>, but the same technique can be adapted to products, sculptures, architectural models, and other 3D objects.</p>
<h2>✨ Features</h2>
<h3>GPU-Powered Sand Disintegration</h3>
<ul>
<li>
<p>140,000 particles forming the demonstration model.</p>
</li>
<li>
<p>Area-weighted triangle sampling for particle distribution.</p>
</li>
<li>
<p>Barycentric coordinates for placing particles on mesh surfaces.</p>
</li>
<li>
<p>Individual displacement vectors and randomized animation delays.</p>
</li>
<li>
<p>Smooth, reversible transitions between assembled and scattered states.</p>
</li>
<li>
<p>Procedural particle movement calculated in GLSL vertex shaders.</p>
</li>
<li>
<p>No need to update all particle positions on the CPU every frame.</p>
</li>
</ul>
<h3>Interactive 3D Camera</h3>
<ul>
<li>
<p>Mouse and touch-based rotation using OrbitControls.</p>
</li>
<li>
<p>Zoom controls with configurable distance limits.</p>
</li>
<li>
<p>Smooth camera damping.</p>
</li>
<li>
<p>Automatic disintegration when the camera moves.</p>
</li>
<li>
<p>Gradual reconstruction when camera movement stops.</p>
</li>
</ul>
<p>The effect can also be adapted to respond to scrolling, buttons, sliders, GSAP animations, or other application events.</p>
<h3>Procedural Visual Effects</h3>
<p>To enhance the demonstration, I added several optional effects:</p>
<ul>
<li>
<p><strong>22,000 orbital particles</strong> forming a rotating ring of cosmic dust.</p>
</li>
<li>
<p><strong>1,400 background stars</strong> creating a sense of depth.</p>
</li>
<li>
<p>Procedural lightning bolts with branching paths.</p>
</li>
<li>
<p>Blue, green, and pink electrical discharges.</p>
</li>
<li>
<p>Energy waves propagating around the orbital ring.</p>
</li>
<li>
<p>Animated energy patterns generated with procedural noise.</p>
</li>
<li>
<p>Dynamic particle colors and additive blending.</p>
</li>
</ul>
<p>These effects are part of the demonstration, not requirements of the core disintegration system.</p>
<h3>Color Customization</h3>
<p>The demonstration includes a color picker that allows visitors to change the appearance of the assembled logo.</p>
<p>The selected color is saved using <code>localStorage</code> and restored on subsequent visits when browser storage is available.</p>
<h2>🧠 How It Works</h2>
<p>The main effect combines CPU-based geometry preparation with GPU-based animation.</p>
<p><strong>1. Mesh sampling</strong></p>
<p>I start with two arrays: vertex positions and triangle indices.</p>
<p>For each triangle, I calculate its surface area and normal vector. I then use area-weighted random selection and barycentric coordinates to distribute particles across the model.</p>
<p><strong>2. Particle initialization</strong></p>
<p>Every particle receives its original position, a displacement vector, a random animation delay, and additional attributes used by the shaders.</p>
<p><strong>3. GPU animation</strong></p>
<p>I pass the particle data to <code>THREE.BufferGeometry</code> and render it using <code>THREE.Points</code> with a custom <code>ShaderMaterial</code>.</p>
<p>A single uniform, <code>uMix</code>, controls the transition:</p>
<pre><code>float l = clamp((uMix - aSand.w * 0.55) / 0.45, 0.0, 1.0);
float e = l * l * (3.0 - 2.0 * l);

vec3 p = position + aSand.xyz * e;</code></pre>
<p>When <code>uMix</code> approaches <code>1.0</code>, particles move toward their scattered positions.</p>
<p>When <code>uMix</code> returns to <code>0.0</code>, they move back to their original locations and reconstruct the model.</p>
<p>Because the animation is reversible, I do not need a separate reconstruction algorithm.</p>
<p><strong>4. Interaction</strong></p>
<p>In the demo, I detect camera movement and update <code>uMix</code> accordingly. This creates a direct connection between user interaction and particle behavior.</p>
<h2>🛠 Technologies</h2>
<ul>
<li>
<p>JavaScript</p>
</li>
<li>
<p>Three.js r128</p>
</li>
<li>
<p>WebGL</p>
</li>
<li>
<p>GLSL vertex and fragment shaders</p>
</li>
<li>
<p>HTML5 Canvas</p>
</li>
<li>
<p>OrbitControls</p>
</li>
<li>
<p>Typed arrays and BufferGeometry</p>
</li>
</ul>
<h2>🚀 Getting Started</h2>
<ol start="1">
<li>
<p>Download or clone the repository.</p>
</li>
<li>
<p>Open <code>index.html</code> in a modern browser with WebGL support.</p>
</li>
<li>
<p>Drag to rotate the scene and watch the model disintegrate.</p>
</li>
<li>
<p>Stop moving the camera to see the particles reconstruct the model.</p>
</li>
<li>
<p>Use the color picker to change the assembled model's color.</p>
</li>
</ol>
<p><strong>No npm installation, build tools, backend, or internet connection are required for the supplied version.</strong></p>
<p>Three.js, OrbitControls, and the demonstration model are included locally.</p>
<h2>📁 Project Structure</h2>
<pre><code>threejs-sand-logo/
├── index.html
├── vendor/
│   ├── three.min.js
│   └── OrbitControls.js
└── js/
    ├── logo-data.js
    └── animation.js</code></pre>
<p>The repository separates the third-party libraries, encoded example model, and custom animation logic.</p>
<h2>⚙️ Customization</h2>
<p>Several parameters can be adjusted to change the appearance and performance of the scene.</p>
<table>
<tbody>
<tr>
<th>Parameter</th>
<th>Default</th>
<th>Purpose</th>
</tr>
<tr>
<td><code>NL</code></td>
<td>140000</td>
<td>Number of model particles</td>
</tr>
<tr>
<td><code>NR</code></td>
<td>22000</td>
<td>Number of orbital ring particles</td>
</tr>
<tr>
<td><code>R0</code></td>
<td>62</td>
<td>Main orbital radius</td>
</tr>
<tr>
<td><code>ns</code></td>
<td>1400</td>
<td>Number of background stars</td>
</tr>
<tr>
<td><code>SL</code></td>
<td>4</td>
<td>Lightning event slots</td>
</tr>
<tr>
<td><code>uMix</code></td>
<td>0–1</td>
<td>Disintegration and reconstruction progress</td>
</tr>
<tr>
<td><code>dampingFactor</code></td>
<td>0.12</td>
<td>Camera movement damping</td>
</tr>
</tbody>
</table>
<p>Reducing particle counts can improve performance on lower-powered devices.</p>
<h2>🔄 Using a Different 3D Model</h2>
<p>Although the demonstration features my Design Cart logo, the particle sampling algorithm is not tied to its shape.</p>
<p>To adapt the effect to another object, I can provide a different set of triangle vertex positions and indices.</p>
<p>The input mesh can originate from a <code>THREE.BufferGeometry</code> object or be prepared from a 3D asset such as GLTF, GLB, or OBJ.</p>
<p>Depending on the new model, I may also need to adjust:</p>
<ul>
<li>
<p>Mesh scale and centering.</p>
</li>
<li>
<p>Particle displacement distances.</p>
</li>
<li>
<p>Sampling weights and distribution.</p>
</li>
<li>
<p>Camera position and zoom limits.</p>
</li>
<li>
<p>Orbital ring dimensions.</p>
</li>
<li>
<p>Lightning start and end positions.</p>
</li>
</ul>
<p>The current implementation reconstructs the same input shape. Morphing between two different models would require additional particle correspondence logic.</p>
<h2>💡 Possible Applications</h2>
<p>I see several practical uses for this technique:</p>
<ul>
<li>
<p>Interactive landing pages and hero sections.</p>
</li>
<li>
<p>3D product presentations.</p>
</li>
<li>
<p>Architecture and building visualizations.</p>
</li>
<li>
<p>Creative portfolios.</p>
</li>
<li>
<p>Hologram and teleportation effects.</p>
</li>
<li>
<p>Science-fiction interfaces.</p>
</li>
<li>
<p>Game effects and object materialization.</p>
</li>
<li>
<p>Scroll-driven animations and cinematic transitions.</p>
</li>
</ul>
<p>The core principle remains the same: sample a 3D mesh, animate its particles on the GPU, and restore the original geometry whenever needed.</p>
<h2>👨‍💻 Author</h2>
<p>Developed by <strong>Paweł Nosko — Design Cart</strong>.</p>
<p>I'm a web developer working on e-commerce systems, custom extensions, JavaScript applications, and interactive web experiences.</p>
<ul>
<li>
<p><a href="https://pawelnosko.com/js-frontend-tools/jak-zamienic-model-3d-w-piasek-interaktywny-rozpad-i-rekonstrukcja-obiektow-w-three-js-i-webgl">Project Website</a></p>
</li>
<li>
<p><a href="https://www.designcart.pl/pawel-nosko.html">Paweł Nosko — Author Profile</a></p>
</li>
</ul>
<h2>Third-Party Libraries</h2>
<p>This project includes Three.js r128 and OrbitControls from the Three.js project. Their respective copyright notices and MIT licensing information are retained in the supplied library files.</p>
<h2>⭐ Support the Project</h2>
<p>If you find this experiment useful, consider starring the repository.</p>
<p>Explore the source code, experiment with different geometries, modify the shaders, and create your own interactive particle effects.</p>
<p><strong>One particle system. Different 3D models. Endless visual possibilities. ⚡</strong></p>
