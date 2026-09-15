# Bosch DINION 7100i IR (2MP / NBE-7702) Field of View & Distance Tool

Interactive client-side Field of View (FOV) and distance estimation engine for the **Bosch DINION 7100i IR (2MP / NBE-7702)** camera. Hosted natively via GitHub Pages at:
`https://sureshmagix.github.io/wii-dinion-fov-tool/`

---

## Supported Camera Specifications

The application models the native 1/1.8" CMOS sensor ($1920 \times 1080$, 16:9 aspect ratio) across both factory lens configurations:

| Parameter | Standard Lens (NBE-7702-ALX) | Telephoto Lens (NBE-7702-ALXT) |
| :--- | :--- | :--- |
| **Focal Length** | 4.7 mm – 10 mm | 10.5 mm – 47 mm |
| **Horizontal FOV (HFOV)** | $103^\circ \to 49^\circ$ | $41.6^\circ \to 9.3^\circ$ |
| **Vertical FOV (VFOV)** | $53^\circ \to 27^\circ$ | $23.9^\circ \to 5.3^\circ$ |
| **Native Resolution** | $1920 \times 1080$ | $1920 \times 1080$ |

---

## Core Mathematical Formulas

### 1. Optical Geometry & Ground Footprint

* **Focal Length in Pixels ($f_{px}$):**
  Derived from the field of view angles and frame dimensions ($W_{px} = 1920$, $H_{px} = 1080$):
  $$f_x = \frac{W_{px} / 2}{\tan(\text{HFOV} / 2)}, \quad f_y = \frac{H_{px} / 2}{\tan(\text{VFOV} / 2)}$$
  $$f_{px} = \frac{f_x + f_y}{2}$$

* **Horizontal Ground Footprint Width ($W_{scene}$):**
  The lateral field coverage at target distance $D$ along the optical center line:
  $$W_{scene} = 2 \times D \times \tan\left(\frac{\text{HFOV}}{2}\right)$$

* **Vertical Ground Footprint Height ($H_{scene}$):**
  $$H_{scene} = 2 \times D \times \tan\left(\frac{\text{VFOV}}{2}\right)$$

---

### 2. Mounting Height, Tilt & Blind Spot (Dead Zone)

Given a mounting height $h_{mount}$ and a downward tilt angle $\alpha$:

$$\theta_{\text{top}} = \alpha - \frac{\text{VFOV}}{2}, \quad \theta_{\text{bottom}} = \alpha + \frac{\text{VFOV}}{2}$$

* **Near Ground Cutoff (Blind Zone Boundary under the pole):**
  $$\text{Cutoff}_{\text{near}} = \frac{h_{mount}}{\tan(\theta_{\text{bottom}})}$$

* **Far Ground Cutoff:**
  $$\text{Cutoff}_{\text{far}} = \frac{h_{mount}}{\tan(\theta_{\text{top}})} \quad (\text{valid for } \theta_{\text{top}} > 0)$$

---

### 3. DORI & Pixel Density Standards (EN 62676-4)

Pixels-Per-Meter ($\text{PPM}$) indicates the level of detail available at distance $D$ for a $1920 \times 1080$ sensor:

$$\text{PPM} = \frac{W_{px}}{W_{scene}} = \frac{1920}{2 \times D \times \tan(\text{HFOV} / 2)}$$

The industry DORI threshold criteria:
* **Detection ($\ge 25\text{ PPM}$):** Verify whether a human or vehicle is present.
* **Observation ($\ge 63\text{ PPM}$):** View characteristic details (clothing color, vehicle shape).
* **Recognition ($\ge 125\text{ PPM}$):** Determine with certainty whether an individual has been seen before.
* **Identification ($\ge 250\text{ PPM}$):** Enable identification of an individual beyond reasonable doubt.

---

### 4. Real-Time Distance Estimation Methods

#### Method A: Native Bosch IVA Calibrated 3D Coordinates
When Bosch 3D Scene Calibration is active in camera firmware, the embedded ONVIF metadata stream outputs Cartesian ground-plane coordinates $(X, Y, Z)$ relative to the pole base:

* **Ground Plane Distance:**
  $$D_{\text{ground}} = \sqrt{X^2 + Y^2}$$

* **Direct Line-of-Sight (Euclidean) Distance:**
  $$D_{\text{LOS}} = \sqrt{X^2 + Y^2 + Z^2}$$

* **Azimuth Angle ($\theta$):**
  $$\theta = \arctan2(X, Y)$$

#### Method B: Pinhole Triangle Similarity (Fallback / ROI Tracker)
When evaluating custom bounding boxes using known target heights ($H_{\text{real}}$):

* **Line-of-Sight Distance ($D_{\text{LOS}}$):**
  $$D_{\text{LOS}} = \frac{H_{\text{real}} \times f_{px}}{h_{px}}$$
  *(where $h_{px}$ is the vertical height of the bounding box in pixels and $f_{px}$ is effective focal length)*.

* **Projected Ground Distance ($D_{\text{ground}}$):**
  $$D_{\text{ground}} = \sqrt{\max\left(0, D_{\text{LOS}}^2 - h_{mount}^2\right)}$$

---

## Object Reference Presets

For optical similarity estimations, standard height baselines ($H_{\text{real}}$) are mapped as follows:

* **Standing Human:** $1.70\text{ m}$
* **Seated Human:** $1.30\text{ m}$
* **Sedan / Standard Car:** $1.48\text{ m}$
* **SUV / Van:** $1.80\text{ m}$
* **Truck / Bus:** $3.20\text{ m}$
* **Bicycle & Rider:** $1.50\text{ m}$
* **Standard Door Height:** $2.05\text{ m}$

---

## Local Development & Deployment

Clone the repository and preview the visualizer locally:

```bash
git clone [https://github.com/sureshmagix/wii-dinion-fov-tool.git](https://github.com/sureshmagix/wii-dinion-fov-tool.git)
cd wii-dinion-fov-tool

# Open with any local web server or browser
open index.html