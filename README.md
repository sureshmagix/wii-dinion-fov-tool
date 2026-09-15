# Universal Camera Field of View (FOV) & 2D/3D Planning Tool

An interactive, high-precision optical geometry, 2D plan, and 3D frustum visualization suite for CCTV, surveillance, and computer vision cameras.

Hosted via GitHub Pages:
**`https://sureshmagix.github.io/wii-dinion-fov-tool/`**

---

## Key Features

- **Universal Camera & Sensor Support**:
  - Pre-configured camera profiles: Bosch DINION 7100i IR (Standard ALX & Telephoto ALXT lenses), Generic 1080p Fixed Dome & Bullet, Generic 4MP Varifocal, Generic 4K/8MP Ultra HD, and Custom Camera specification.
  - Sensor formats: 1/1.8", 1/2.8", 1/2.7", 1/2.5", 1/2", 1/3", 2/3", 1", and custom sensor width/height in millimeters.
  - Resolutions: 1080p (1920×1080), 4MP (2560×1440), 5MP (2592×1944), 4K/8MP (3840×2160), 12MP (4000×3000), 720p, or custom pixel dimensions.

- **Bidirectional Optical Synchronization**:
  - Live reactive conversion between **Focal Length ($f$ in mm)** and **Field of View angles** (Horizontal HFOV, Vertical VFOV, and Diagonal DFOV).

- **Multi-Angle & 3D Frustum Visualizations**:
  - **Combined Multi-View**: Synchronized quad-panel view displaying 3D spatial frustum, simulated camera viewfinder, 2D top-down ground plan, and 2D side elevation profile simultaneously.
  - **Interactive 3D Frustum & Ground Footprint (WebGL / Three.js)**:
    - Orbit, pan, and zoom around a 3D metric scene with detailed CCTV security camera housing, mast pole, and mounting plate.
    - True mathematical ground intersection footprint polygon ($Y = 0$) with glowing borders.
    - Concentric DORI colored coverage zones (Identification, Recognition, Observation, Detection) mapped directly on the ground plane.
    - CAD-grade 3D measurement dimension lines with camera-facing billboard badges: Mount Height ($h$), Blind Spot near cutoff under the pole, Target Ground Distance ($D$), Direct Line-of-Sight (LOS) hypotenuse ray, and Field Width ($W_{\text{scene}}$) bar.
    - Procedural 3D target models: Standing Person (1.70m), Seated Person with chair (1.30m), Sedan Car (1.48m), Full-size SUV / Van (1.80m), Heavy Commercial Truck / Bus (3.20m), License Plate (0.15m), and Custom Target with circular landing pad.
    - 1-click camera angle presets: 3D Isometric, Top View, Side View, and Camera POV looking through the lens.
  - **2D Plan View (Top-Down Azimuth Footprint)**: Radial distance range rings, concentric DORI coverage sectors, camera pan heading arc, blind spot radius, field width measurement bar, and architectural blueprint symbols for pedestrian, seated, car, SUV, truck, and plate.
  - **2D Side Profile (Elevation & Blind Spot)**: Optical axis tilt angle arc, upper/lower ray trajectories, shaded hazard dead zone directly under the pole, direct LOS ray, and accurate target side silhouettes with metric height dimension line.
  - **Simulated Camera Viewfinder & Inspector**:
    - Pinhole sensor viewport with live CCTV OSD (timecode, camera tag, optical specs, horizon line, crosshair reticle).
    - Authentic scalable vector artwork matching selected target object (person in clothing/vest, seated human, sedan car, SUV, truck, or license plate) scaled by distance perspective.
    - Target detail loupe with simulated 1:1 sensor pixel grid and EN 62676-4 compliance checklist.
    - Reticle corner brackets `[  ]` and floating HUD tag showing pixel dimensions and DORI level.

- **EN 62676-4 / IEC 62676-4 DORI Compliance**:
  - Live Pixels-Per-Meter ($\text{PPM}$) and Pixels-Per-Foot ($\text{PPF}$) density estimation at target distance.
  - Automatic DORI classification:
    - **Identification ($\ge 250\text{ PPM}$)**
    - **Recognition ($\ge 125\text{ PPM}$)**
    - **Observation ($\ge 63\text{ PPM}$)**
    - **Detection ($\ge 25\text{ PPM}$)**
    - **Monitoring ($< 25\text{ PPM}$)**
  - Maximum effective range table for every DORI threshold.
  - Real-time target compliance assessment panel.

- **Configuration Import / Export & Reporting**:
  - **1-Click Snapshot Export**: High-resolution PNG image download of 3D frustum, viewfinder, plan, or elevation views for security submittals and engineering reports.
  - **Export JSON**: Download `.json` configuration file containing all camera optics, sensor specs, mounting parameters, and target dimensions.
  - **Import JSON**: File picker or instant drag-and-drop `.json` file anywhere onto the web page.
  - **Browser Preset Storage**: Save, load, and delete custom named site setups directly in `localStorage`.
  - **Metric & Imperial**: Seamless 1-click toggle between Meters ($\text{m}$) and Feet ($\text{ft}$).

---

## Core Optical & Mathematical Formulations

### 1. Sensor Geometry & Bidirectional FOV Calculation

Given sensor physical dimensions ($W_{\text{sensor}}$ and $H_{\text{sensor}}$ in mm) and lens focal length ($f$ in mm):

$$\text{HFOV} = 2 \times \arctan\left(\frac{W_{\text{sensor}}}{2f}\right) \times \left(\frac{180^\circ}{\pi}\right)$$

$$\text{VFOV} = 2 \times \arctan\left(\frac{H_{\text{sensor}}}{2f}\right) \times \left(\frac{180^\circ}{\pi}\right)$$

$$\text{DFOV} = 2 \times \arctan\left(\frac{\sqrt{W_{\text{sensor}}^2 + H_{\text{sensor}}^2}}{2f}\right) \times \left(\frac{180^\circ}{\pi}\right)$$

Conversely, given a desired Horizontal FOV angle ($\text{HFOV}$):

$$f = \frac{W_{\text{sensor}}}{2 \times \tan\left(\frac{\text{HFOV}}{2} \times \frac{\pi}{180^\circ}\right)}$$

---

### 2. Scene Coverage & Pixel Density (EN 62676-4 DORI)

At ground distance $D$ along the optical center line:

* **Horizontal Field Width ($W_{\text{scene}}$):**
  $$W_{\text{scene}} = 2 \times D \times \tan\left(\frac{\text{HFOV}}{2}\right)$$

* **Vertical Field Height ($H_{\text{scene}}$):**
  $$H_{\text{scene}} = 2 \times D \times \tan\left(\frac{\text{VFOV}}{2}\right)$$

* **Pixels-Per-Meter ($\text{PPM}$):**
  $$\text{PPM} = \frac{W_{\text{px}}}{W_{\text{scene}}} = \frac{W_{\text{px}}}{2 \times D \times \tan\left(\frac{\text{HFOV}}{2}\right)}$$

* **Maximum DORI Distance Range for Threshold $PPM_{\text{target}}$:**
  $$D_{\text{max}} = \frac{W_{\text{px}}}{2 \times PPM_{\text{target}} \times \tan\left(\frac{\text{HFOV}}{2}\right)}$$

---

### 3. Mounting Height, Tilt & Ground Cutoffs (Blind Spot)

Given a mounting height $h$ and a downward tilt angle $\alpha$:

$$\theta_{\text{top}} = \alpha - \frac{\text{VFOV}}{2}, \quad \theta_{\text{bottom}} = \alpha + \frac{\text{VFOV}}{2}$$

* **Near Blind Spot / Dead Zone Boundary (under pole):**
  $$\text{Cutoff}_{\text{near}} = \frac{h}{\tan(\theta_{\text{bottom}})} \quad \left(0 < \theta_{\text{bottom}} < \frac{\pi}{2}\right)$$

* **Far Ground Cutoff:**
  $$\text{Cutoff}_{\text{far}} = \frac{h}{\tan(\theta_{\text{top}})} \quad (\theta_{\text{top}} > 0)$$
  *(When $\theta_{\text{top}} \le 0$, the camera's upper field of view extends to or above the horizon).*

---

### 4. 3D Spatial Coordinates & Line-of-Sight

* **Direct Line-of-Sight (Euclidean) Distance:**
  $$D_{\text{LOS}} = \sqrt{D_{\text{ground}}^2 + h^2 + X_{\text{offset}}^2}$$

* **Target Object Pixel Height in Sensor Frame ($h_{\text{px}}$):**
  $$h_{\text{px}} = \frac{H_{\text{target}} \times f_{\text{px}}}{D_{\text{LOS}}}$$
  where $f_{\text{px}} = \frac{W_{\text{px}} / 2}{\tan(\text{HFOV}/2)}$.

---

## JSON Configuration Schema

Exported configuration files adhere to the following schema:

```json
{
  "schema": "wii-cctv-fov-planner/v1",
  "exportedAt": "2026-09-15T10:00:00.000Z",
  "camera": {
    "label": "CAM-01 DINION 7100i",
    "preset": "bosch-7100i-wide",
    "sensor": {
      "format": "1/1.8",
      "widthMm": 7.18,
      "heightMm": 5.32
    },
    "resolution": {
      "preset": "1920x1080",
      "widthPx": 1920,
      "heightPx": 1080,
      "aspectRatio": "16:9"
    },
    "optics": {
      "focalLengthMm": 4.7,
      "hfovDeg": 75.0,
      "vfovDeg": 45.0,
      "dfovDeg": 85.0
    }
  },
  "mounting": {
    "heightM": 4.0,
    "tiltAngleDeg": 15.0,
    "panAzimuthDeg": 0.0
  },
  "target": {
    "type": "human",
    "heightM": 1.70,
    "groundDistanceM": 25.0,
    "lateralOffsetM": 0.0
  },
  "metrics": {
    "pixelsPerMeter": 55,
    "doriClassification": "Observation",
    "groundWidthM": 35.0,
    "blindSpotNearCutoffM": 5.1,
    "farCutoffM": 74.2,
    "doriRangesM": {
      "identification": 5.5,
      "recognition": 11.0,
      "observation": 21.8,
      "detection": 54.9
    }
  }
}
```

---

## Local Development & Usage

Clone the repository and open `index.html` in any modern web browser:

```bash
git clone https://github.com/sureshmagix/wii-dinion-fov-tool.git
cd wii-dinion-fov-tool

# Open directly in your browser
open index.html
```

Deploying to GitHub Pages requires no build step—simply push `index.html` to the `main` branch.