# CineChroma — Cinematic 60/30/10 Color Analyzer

## 1. Introduction

The classical 60/30/10 color balance rule in cinematography defines the relative area proportions of the dominant, secondary, and accent colors within a scene. **CineChroma** is a browser-based computer vision application developed to quantitatively test this principle in film frames. The software analyzes user-uploaded key frames, reports the dominant, secondary, and accent colors together with their pixel coverage ratios, and compares these ratios against the 60/30/10 targets.

## 2. Software Overview

CineChroma runs entirely in the user's own browser; images are never transmitted to a server. The application offers two analysis methods:

1. **Hybrid Color-Based**
2. **CIELAB-Based**

Users can adjust parameters such as sampling density, optional noise reduction (DMF), and palette size (3, 4, or 5 colors). Results are presented with the hex code, HSV values, pixel coverage, target deviation, and correlated color temperature (CCT) for each color. Results can be exported as Excel (.xlsx), and PNG outputs visualizing the color proportions can be generated individually or in batch.

## 3. Method

### 3.1 Hybrid Color-Based

The Hybrid Color-Based method combines HSL color-family pre-classification with pixel assignment based on perceptual distance in CIELAB space. The image is first downscaled to preserve analysis speed; an optional Dominant-Pixels based Mean Filter (DMF) reduces local pixel noise. RGB pixels are assigned to red, orange, yellow, green, teal, blue, purple, black, and white/gray families. For chromatic families, pixels with high saturation and mid-range lightness are given priority when computing representative colors.

Representative colors are converted into CIE 1976 L*a*b* space, which supports a perceptually more meaningful evaluation of color differences than RGB (Connolly & Fleiss, 1997). The largest family is selected as the dominant color; secondary and accent colors are chosen from candidates sufficiently distinct from the selected centers in CIELAB distance. Each pixel is assigned to its nearest representative, which can be interpreted as a Voronoi-type nearest-center partitioning (Lloyd, 1982). For low-saturation images, a CIELAB K-Means fallback prevents black, gray, and white from merging into a single family. Results are compared against the 60/30/10 targets; correlated color temperature (CCT) is provided only as a supplementary indicator (McCamy, 1992).

### 3.2 CIELAB-Based

The CIELAB-Based method identifies dominant colors in key frames using the K-Means clustering algorithm in CIE 1976 L*a*b* space, in a data-driven and reproducible manner. Cluster centers are initialized with K-Means++, which selects starting centers that are spread apart, reducing the instability that can result from arbitrary initialization (Arthur & Vassilvitskii, 2007). Each pixel is assigned to its nearest center; centers are updated as the mean L*a*b* value of assigned pixels. This process continues until assignments stabilize or a maximum of 25 iterations is reached.

Two representation modes are offered for each cluster: "Mean" converts the cluster center back to RGB, yielding the theoretical average color; "Real Pixel" reports the actual source pixel with the smallest CIELAB distance to the center (Park & Jun, 2009). Clusters are ranked by pixel coverage; the top three are assigned as dominant, secondary, and accent colors and compared against the 60/30/10 targets. The effectiveness of K-Means for color quantization has been examined in detail by Celebi (2011).

## 4. Validity and Verification

The validity of the software's color-proportion output was examined using two complementary approaches. First, synthetic reference images with a precisely known ground-truth color ratio were created in Adobe Photoshop. These consisted of a chromatic color bar in which the three primary colors (RGB: red, green, blue) were arranged in a 60/30/10 ratio, and an achromatic bar with the same ratio using black, gray, and white. Such synthetic, exact-ratio test images represent a widely used known-answer testing strategy for evaluating image-processing algorithms, since the expected output is known in advance, allowing systematic errors to be detected directly rather than inferred (Jannin et al., 2002). The software's sensitivity parameters—sampling density, DMF setting, and palette size—were tuned and verified against these reference images until the software reproduced the true 60/30/10 ratio.

Second, the same reference bars and additional test images were independently analyzed using an external, browser-based dominant-color extraction service, imageonline.io/dominant-colors, which reports percentage-based three-color palettes. Comparing the outputs of an independently developed tool against the outputs of the present software constitutes a criterion-validity check, indicating the degree of agreement between the measurement instrument and an external reference (Boita et al., 2021). A comparable validation logic has been applied in prior work showing that K-means–based dominant-color extraction produces consistent, low-variance results across repeated runs and image sets (Sánchez-Sánchez et al., 2020).

Taken together, these two independent verification steps support the conclusion that the software computes 60/30/10 color proportions with adequate accuracy. However, this verification is based on a limited number of controlled test images; a broader, systematic validation across diverse image types, compression formats, and lighting conditions would further strengthen the generalizability of the method.

## 5. Interface and Usage

The CineChroma interface operates in both Turkish and English. The main screen provides method selection, sampling density, DMF, and palette-size controls; the top-right corner hosts buttons for clearing results, exporting to Excel, and generating batch analysis images. The "Method" window presents the academic description of both techniques along with the corresponding PDF documents; the "Validity" window provides the verification process summarized above as text. Each analysis result can optionally be exported as a PNG with a color-proportion bar (vertical or horizontal) overlaid within the original photo boundaries.

## 6. Limitations and Future Work

The software's validation relies on a limited number of controlled synthetic test images and a single external comparison tool. Future work should pursue systematic validation using a broader reference dataset spanning diverse image types, compression formats, and lighting conditions. Additionally, applying more advanced color-difference formulas such as CIEDE2000 may further improve perceptual accuracy.

## References (APA 7)

Arthur, D., & Vassilvitskii, S. (2007). *k-means++: The advantages of careful seeding*. In *Proceedings of the Eighteenth Annual ACM-SIAM Symposium on Discrete Algorithms* (pp. 1027–1035). Society for Industrial and Applied Mathematics. https://doi.org/10.5555/1283383.1283494

Boita, J., et al. (2021). Validation of a candidate instrument to assess image quality in digital mammography. *European Journal of Radiology, 145*, Article 110022. https://doi.org/10.1016/j.ejrad.2021.110022

Celebi, M. E. (2011). Improving the performance of k-means for color quantization. *Image and Vision Computing, 29*(4), 260–271. https://doi.org/10.1016/j.imavis.2010.10.002

Connolly, C., & Fleiss, T. (1997). A study of efficiency and accuracy in the transformation from RGB to CIELAB color space. *IEEE Transactions on Image Processing, 6*(7), 1046–1048. https://doi.org/10.1109/83.597279

Jannin, P., Fitzpatrick, J. M., Hawkes, D. J., Pennec, X., Shahidi, R., & Vannier, M. W. (2002). Validation of medical image processing in image-guided therapy. *IEEE Transactions on Medical Imaging, 21*(12), 1445–1449. https://doi.org/10.1109/TMI.2002.806568

Lloyd, S. P. (1982). Least squares quantization in PCM. *IEEE Transactions on Information Theory, 28*(2), 129–137. https://doi.org/10.1109/TIT.1982.1056489

McCamy, C. S. (1992). Correlated color temperature as an explicit function of chromaticity coordinates. *Color Research & Application, 17*(2), 142–144. https://doi.org/10.1002/col.5080170211

Park, H.-S., & Jun, C.-H. (2009). A simple and fast algorithm for K-medoids clustering. *Expert Systems with Applications, 36*(2), 3336–3341. https://doi.org/10.1016/j.eswa.2008.01.039

Sánchez-Sánchez, C., et al. (2020). Dominant color extraction with K-means for camera characterization in cultural heritage documentation. *Remote Sensing, 12*(3), Article 520. https://doi.org/10.3390/rs12030520

## Software Citation

Apaydın, C. (2026). *Cinematic color analysis kmeans* [Computer software]. GitHub. https://zenodo.org/records/22102633
