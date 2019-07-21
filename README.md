# DeepFractal

Deep mandelbrot viewer in [webapp](https://munrocket.github.io/deep-fractal/). It uses WebGL and perturbation theory acceleration.

### Features

- [X] Deep zoom (beta version 10^-31, 1024 iteration)
- [X] Webgl parallel processing
- [X] Perturbation theory
- [X] Adaptive super-sampling
- [X] Progressive web application
- [X] Julia set and orbits with Ctrl key

![Deep Mandelbrot](https://i.imgur.com/EfIDzxt.png)

### How it works

1. Since each pixel in a fractal is calculated independantly we can parallize it with GPU.
2. Using the perturbation method, we can calculate only one point with an arbitrary precision in CPU
and restore the rest of the picture with float precision in GPU.
   - Finding a new reference point *O* (origin) where orbit sequence *O(n) < B* and calculate it with arbitrary precision.
   - Pass truncated to float precision orbit sequence to GPU and restore rest of points *Z(n) = O(n) + W(n)* with equation
     *W(n+1) -> W(n)^2 + 2 * O(n) * W(n) + delta*, where *delta* is coordinates relative to the new coordinate system and *Z(0) = O(0) + delta*.
3. Since this orbit sequence too big for GPU uniforms we need to use ping-pong rendering.
4. Using arbitrary precision library based on *floating point expation* which is heavily optimized for geometric calculation.

### 2do
- [X] Double.js as a library for arbitrary precision
- [X] Ping-pong rendering for deeper zoom
- [X] Logarithmic search for reference point
- [ ] Better optimization (SDF serach, progressive render, bigfloat)
- [ ] New feature (internal coord, router, save picture button)

### Lighthouse report
![audit](https://i.imgur.com/RweUezL.png?1)

### References

1. Bruce Dawson. *Faster Fractals Through Algebra*. [[url](https://randomascii.wordpress.com/2011/08/13/faster-fractals-through-algebra/)]
2. Jussi Harkonen. *On Smooth Fractal Coloring Techniques*. [[pdf](http://jussiharkonen.com/files/on_fractal_coloring_techniques(lo-res).pdf)]
3. Javier Barrallo, Damien Jones. *Coloring algorithms for dynamical systems in the complex plane*. [[pdf](http://math.unipa.it/~grim/Jbarrallo.PDF)]
4. K.I. Martin. *Superfractalthing math.* [[pdf](http://www.superfractalthing.co.nf/sft_maths.pdf)]
5. Robert Munafo. *Speed Improvements* [[url](https://mrob.com/pub/muency/speedimprovements.html)]
6. Claude Heiland-Allen. *Adaptive super-sampling using distance estimate.* [[url](http://mathr.co.uk/blog/2014-11-22_adaptive_supersampling_using_distance_estimate.html)]
7. Arnaud Cheritat. *Mandelbrot set* [[url](https://www.math.univ-toulouse.fr/~cheritat/wiki-draw/index.php/Mandelbrot_set)]
8. Heinz-Otto Peitgen, Dietmar Saupe, Benoit Mandelbrot etc. *The Science of Fractal Images.* Appendix D.

[//]: # "*Numerical Methods for Finding Periodic Orbits* [[url](http://www.scholarpedia.org/article/Periodic_orbit#Numerical_Methods_for_Finding_Periodic_Orbits)]"
[//]: # "*Practical interior distance rendering* http://mathr.co.uk/blog/2014-11-02_practical_interior_distance_rendering.html"
[//]: # "https://mathr.co.uk/mandelbrot/book-draft-2017-11-10.pdf"
[//]: # "http://roy.red/fractal-droste-images-.html#fractal-droste-images"
[//]: # "http://ibiblio.org/e-notes/MSet/ru/cont_r.htm"
[//]: # " Posible coloring: gaussian integer distance
          Intresting modifications:
            drop: z -> z^2 + 1/c
            eye: z -> z^3 + 1/c
            circle: z -> z^2 + 1/c - 1
            stripe: z -> z^2 + 1/(conj(c) - 0.5) - 3/4
            mandelpinski: julia z -> z^4 - 0.1/z^4"
