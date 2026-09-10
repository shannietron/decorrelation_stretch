# Decorrelation Stretch

A single-file, dependency-free web tool that applies a **decorrelation stretch** to images
and to a live camera feed, entirely in the browser. Nothing is uploaded; no image data
leaves the page.

**Live:** https://shannietron.github.io/decorrelation_stretch/

Decorrelation stretching is the technique behind the vivid false-colour Mars and ASTER
images: the three colour bands are rotated into their principal axes, each axis is
stretched to equal variance, and the result is rotated back. Subtle colour differences
that were hidden by strong band-to-band correlation become clearly separated hues.

## Use

Open `index.html` directly, or use the live link above.

- **Images tab.** Drop, paste, or pick one or more images. Each card shows the original
  and the stretched result, a hold-to-compare button, a PNG download, a statistics report
  (band means and sigmas, correlation and covariance matrices, eigenvalues, eigenvectors,
  final transform), and before/after band scatter plots.
- **Live camera tab.** Stretches the camera in real time. Statistics come from a small
  thumbnail and are smoothed over time; *Freeze stats* locks the transform so you can pan
  the camera with fixed colour meaning. Needs HTTPS or localhost for camera access.
- **View.** *Side by side* or *Curtain*: drag (or use arrow keys) to sweep a divider between
  original and stretched.
- **Options.** Correlation or covariance matrix; target mean and sigma (NASA default
  127.5 / 50); optional `Tol` percentile clip; sample stride. Presets for the NASA/ASTER
  convention and for MATLAB's default of preserving each band's own mean and sigma.

## Method

Implements the algorithm in R. E. Alley, *Algorithm Theoretical Basis Document for
Decorrelation Stretch* (ASTER product AST06), v2.2, Jet Propulsion Laboratory, August 1996:
strided sampling, covariance/correlation statistics, eigen-decomposition (cyclic Jacobi),
stretch vector `s = target_sigma / sqrt(lambda)`, transform `T = R diag(s) R^T`, and an offset
placing the output at the target mean. Option semantics follow MATLAB's `decorrstretch`.

## References

- ASTER Decorrelation Stretch product page (JPL):
  https://asterweb.jpl.nasa.gov/content/03_data/01_Data_Products/release_decorrelation_stretch_2_5.htm
- MathWorks, `decorrstretch`: https://www.mathworks.com/help/images/ref/decorrstretch.html
- Gillespie, Kahle & Walker (1986), *Color enhancement of highly correlated images. I. Decorrelation
  and HSI contrast stretches*, Remote Sensing of Environment 20:209–235.
