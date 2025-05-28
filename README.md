# Real-to-Cartoon Image Translation with CycleGAN

Transform real-world photos into beautiful cartoon-style images using deep learning.  
This project implements an unpaired image-to-image translation pipeline using **CycleGAN**, trained to map real photos to the style of a chosen cartoon domain (e.g., Studio Ghibli).

##  Project Overview

- **Goal**: Learn a style mapping from real photos to a cartoon domain using unpaired datasets.
- **Model**: CycleGAN (Zhu et al., 2017)
- **Domains**:
  - **Domain A**: Real-world photos (COCO, Unsplash)
  - **Domain B**: Cartoon-style images (e.g., frames from Studio Ghibli)


---

##  Methodology

This project uses **Cycle-Consistent Adversarial Networks (CycleGAN)** to translate images between unpaired domains. The model includes:

- Generator G: `Real → Cartoon`
- Generator F: `Cartoon → Real`
- Discriminators: \( D_A \), \( D_B \)
- **Losses**:
  - Adversarial loss
  - Cycle-consistency loss
  - Identity loss

CycleGAN learns both directions and enforces structure retention through cycle-consistency.

---

## Dataset

###  Real Photos (trainA)
- Source: [COCO](https://cocodataset.org), [Unsplash](https://unsplash.com)
- ~1000 natural photos
- Resized to 256×256

### Cartoon Frames (trainB)
- Source: Screenshots from Studio Ghibli films
- ~1200 frames extracted with `ffmpeg`
- Preprocessing: Resize, crop, normalize


## 5. Results

### Qualitative Examples

The model successfully translates real photos into stylized cartoon images, preserving structural details while applying soft shading, simplified edges, and the aesthetic of the target cartoon domain (e.g., Studio Ghibli).

| Real Photo | Cartoonized Output |
|------------|--------------------|

>  *Images are sampled from the test set using the best model checkpoint.*

---

### Quantitative Evaluation

We assess model performance with the **Fréchet Inception Distance (FID)**, a commonly used metric to evaluate the quality and diversity of generated images relative to a target domain.

| Comparison                             | FID Score ↓ |
|----------------------------------------|-------------|
| Real photos vs Cartoon dataset         | 72.3        |
| Stylized outputs vs Cartoon dataset    | **39.6**    |

> Lower FID indicates closer resemblance to the target style.

---

### Optional: User Study

To gather subjective feedback, a small user study was conducted with 10 participants who rated the stylized outputs based on:
- **Stylistic Accuracy**
- **Image Realism**
- **Semantic Preservation**

**Average Ratings (out of 5):**
- Style Accuracy: 4.2
- Realism: 3.8
- Content Preservation: 4.4

---

### Training Curves

Training stability is tracked using adversarial, cycle-consistency, and identity losses:

<p align="center">
  <img src="results/loss_plot.png" alt="Training Loss Curve" width="500"/>
</p>

---

### Observations

- The model performs well on scenes with clear structure (e.g. buildings, forests).
- Some artifacts appear in highly textured or abstract areas.
- Faces are preserved but may lack cartoon-like exaggeration (can be improved with UGATIT).

---

##  Challenges

Training an unpaired image-to-image model like CycleGAN introduces several technical and data-related difficulties:

- **GAN Instability**: Adversarial training is inherently unstable and sensitive to learning rates, weight initialization, and model capacity.
- **Data Domain Gap**: Cartoon frames may have flat colors and simplified shapes, while real-world photos include complex textures and lighting.
- **Color Bleed and Artifacts**: In early epochs, outputs often exhibit unnatural colors, checkerboard patterns, or loss of facial detail.
- **Overfitting to Backgrounds**: If the cartoon dataset lacks diversity, the generator may memorize background patterns rather than learning style features.

> These issues were mitigated through careful data preprocessing, early stopping, and cycle-consistency regularization.



