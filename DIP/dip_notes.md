# Digital Image Processing (MCTI-101) - Comprehensive Notes

## Subject Information
- **Subject Code:** MCTI-101
- **Subject Name:** Digital Image Processing
- **Programme:** M. Tech
- **Semester:** 1
- **Credits:** 3 (L:3, T:0, P:0)
- **Teaching Hours:** 36
- **Course Type:** Programme Core

---

# Chapter 1: Introduction to Digital Image Processing

## 1.1 Fundamental Steps in Digital Image Processing

Digital Image Processing is a method to perform operations on an image to get enhanced image or extract useful information from it. The fundamental steps form a pipeline:

```
Input Image → Acquisition → Preprocessing → Segmentation → 
Representation & Description → Recognition & Interpretation → Output
```

### Step 1: Image Acquisition
**Definition:** The process of capturing or obtaining a digital image from a physical scene.

**Hardware Components:**
- **Sensors:** CCD, CMOS sensors
- **Optical Systems:** Lenses, mirrors
- **Digitizers:** A/D converters

**Mathematical Model:**
```
I(x,y) = ∫∫ f(λ,x,y) × s(λ) dλ
```
Where:
- I(x,y) = Image intensity at position (x,y)
- f(λ,x,y) = Scene radiance as function of wavelength λ and position
- s(λ) = Sensor spectral sensitivity

**Example:**
```
Camera Image Acquisition:
Light → Lens → CCD Array → A/D Converter → Digital Image

Physical Scene: Brightness values [0, ∞)
Digital Image: Discrete values [0, 255] for 8-bit
```

### Step 2: Preprocessing
**Purpose:** Improve image quality and prepare for subsequent processing.

**Common Operations:**
- Noise reduction
- Contrast enhancement  
- Geometric corrections
- Color space conversions

**Example - Noise Reduction:**
```
Original Noisy Image Matrix:
[120 115 118]    After Averaging:    [117 117 117]
[125 110 122] →  [3×3 mean filter] → [117 117 117]
[119 116 121]                        [117 117 117]
```

### Step 3: Segmentation
**Definition:** Partitioning an image into meaningful regions or objects.

**Methods:**
1. **Thresholding**
2. **Edge-based segmentation**
3. **Region-based segmentation**

**Example - Binary Thresholding:**
```
Original Grayscale:        Binary Result (T=128):
[50  75  100]             [0  0  0]
[150 200 175]      →      [1  1  1]
[80  90  110]             [0  0  0]

Rule: if pixel > threshold then 1, else 0
```

### Step 4: Representation & Description
**Feature Extraction Examples:**
- **Boundary descriptors:** Chain codes, Fourier descriptors
- **Regional descriptors:** Area, perimeter, texture measures

**Chain Code Example:**
```
Boundary pixels:    Chain code representation:
(0,0)→(0,1)→(1,1)  Direction codes: 0,1,2,3,4,5,6,7
     ↑    ↓        corresponding to 8 directions
(1,0)←(1,1)        
Result: 0,1,2,3 (clockwise from East)
```

### Step 5: Recognition & Interpretation
**Classification and Understanding:**
- Pattern matching
- Machine learning approaches
- Rule-based systems

## 1.2 Components of an Image Processing System

### Hardware Architecture
```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│   Sensors   │ →  │ Digitization │ →  │  Computer   │
│ (CCD/CMOS)  │    │   Hardware   │    │   System    │
└─────────────┘    └──────────────┘    └─────────────┘
                                              │
┌─────────────┐    ┌──────────────┐          │
│   Display   │ ←  │   Storage    │ ←────────┘
│   Devices   │    │   Systems    │
└─────────────┘    └──────────────┘
```

### Software Components
1. **Low-level Processing:** Noise reduction, contrast enhancement
2. **Mid-level Processing:** Segmentation, object recognition
3. **High-level Processing:** Scene interpretation, knowledge-based analysis

### Detailed Component Analysis

#### Image Sensors
**CCD (Charge-Coupled Device):**
- High quality, low noise
- Sequential readout
- Higher power consumption

**CMOS (Complementary Metal-Oxide-Semiconductor):**
- Lower power, faster readout
- On-chip processing capability
- Less expensive

#### Digitization Process
```
Continuous Signal → Sampling → Quantization → Digital Image

Analog Signal: f(x,y) continuous
Sampling: f(i∆x, j∆y) where ∆x, ∆y are sampling intervals
Quantization: Map continuous amplitudes to discrete levels
```

## 1.3 Image Sampling and Quantization

### Sampling Theory

**Definition:** Converting continuous image f(x,y) into discrete image f(i,j)

**Sampling Grid:**
```
Continuous Domain:           Discrete Domain:
y ↑                         j ↑ 4│ • • • •
  │                           │ 3│ • • • •  
  │                           │ 2│ • • • •
  └──→ x                      │ 1│ • • • •
                              └─┼─┼─┼─┼→ i
                                0 1 2 3
```

**Mathematical Representation:**
```
f(x,y) → f(i∆x, j∆y) → f(i,j)

Where:
- ∆x, ∆y = sampling intervals
- i = 0,1,2,...,M-1 (rows)
- j = 0,1,2,...,N-1 (columns)
```

### Nyquist Sampling Theorem
**Statement:** To avoid aliasing, sampling frequency must be at least twice the highest frequency component.

```
fs ≥ 2fmax

Where:
- fs = sampling frequency
- fmax = maximum frequency in the signal
```

**Aliasing Example:**
```
Under-sampled sine wave appears as lower frequency:
Original: sin(2πf₁t) sampled at fs < 2f₁
Appears as: sin(2πf₂t) where f₂ < f₁
```

### Quantization Process

**Definition:** Converting continuous amplitude values to discrete levels.

**Uniform Quantization:**
```
q = round(f/∆) × ∆

Where:
- q = quantized value
- f = continuous amplitude
- ∆ = quantization step size
```

**Quantization Levels:**
```
8-bit quantization: 2⁸ = 256 levels (0-255)
4-bit quantization: 2⁴ = 16 levels (0-15)
1-bit quantization: 2¹ = 2 levels (0,1) - binary
```

**Example - Quantization Effect:**
```
Original values:    [125.7, 130.2, 135.8, 140.1]
8-bit quantized:    [126,   130,   136,   140]
4-bit quantized:    [128,   128,   136,   136]  (levels: 0,16,32,48...)
```

### Sampling and Quantization Trade-offs

**Image Quality vs Storage:**
```
Image Size: M×N pixels
Quantization: L gray levels
Storage = M × N × log₂(L) bits

Example:
512×512 image, 256 gray levels = 512² × 8 = 2,097,152 bits ≈ 2.1 MB
```

**Resolution Effects:**
```
High Resolution (1024×1024): Fine details, large storage
Medium Resolution (512×512): Good quality, moderate storage  
Low Resolution (256×256): Pixelated, small storage
```

---

# Chapter 2: Digital Image Processing Operations

## 2.1 Pixel Relationships and Distance Metrics

### Pixel Neighborhoods

**4-Neighborhood (N₄):**
```
    │ p₁ │    
────┼────┼────
 p₄ │(x,y)│ p₂  
────┼────┼────
    │ p₃ │    

N₄(p) = {(x±1,y), (x,y±1)}
```

**8-Neighborhood (N₈):**
```
p₈ │ p₁ │ p₅
───┼────┼───
p₄ │(x,y)│ p₂  
───┼────┼───
p₇ │ p₃ │ p₆

N₈(p) = N₄(p) ∪ {(x±1,y±1)}
```

**Diagonal Neighbors (Nᴅ):**
```
Nᴅ(p) = {(x±1,y±1)}
```

### Connectivity Types

#### 4-Connectivity
Two pixels p and q are 4-connected if:
1. q ∈ N₄(p)
2. Both pixels have same gray level value or belong to same value set V

**Example:**
```
Binary Image:        4-connected regions:
1 0 1 0              Region 1: {(0,0), (2,0)}
0 1 0 1              Region 2: {(1,1)}  
1 0 1 0              Region 3: {(3,1)}
                     Region 4: {(0,2), (2,2)}
```

#### 8-Connectivity
Two pixels p and q are 8-connected if:
1. q ∈ N₈(p) 
2. Both pixels have same value or belong to value set V

**Example:**
```
Binary Image:        8-connected regions:
1 0 1 0              Region 1: {(0,0), (2,0), (1,1), (0,2), (2,2)}
0 1 0 1              Region 2: {(3,1)}
1 0 1 0              
```

#### m-Connectivity (Mixed Connectivity)
Two pixels p and q are m-connected if:
1. q ∈ N₄(p), OR
2. q ∈ Nᴅ(p) AND N₄(p) ∩ N₄(q) has no pixels with values in V

**Purpose:** Eliminates ambiguity in 8-connectivity

### Distance Metrics

#### Euclidean Distance (L₂ norm)
```
De(p,q) = √[(x₁-x₂)² + (y₁-y₂)²]
```

**Example:**
```
Point p = (2,3), Point q = (5,7)
De = √[(2-5)² + (3-7)²] = √[9 + 16] = √25 = 5
```

**Distance Circle:** Points at distance r form a circle
```
Points at distance 2:
      •
   •     •
•  (x,y)   •
   •     •
      •
```

#### City Block Distance (L₁ norm)
```
D₄(p,q) = |x₁-x₂| + |y₁-y₂|
```

**Example:**
```
Point p = (2,3), Point q = (5,7)
D₄ = |2-5| + |3-7| = 3 + 4 = 7
```

**Distance Diamond:** Points at distance r form a diamond
```
Points at distance 2:
    •
   • •
  • x •
   • •
    •
```

#### Chessboard Distance (L∞ norm)
```
D₈(p,q) = max(|x₁-x₂|, |y₁-y₂|)
```

**Example:**
```
Point p = (2,3), Point q = (5,7)
D₈ = max(|2-5|, |3-7|) = max(3,4) = 4
```

**Distance Square:** Points at distance r form a square
```
Points at distance 2:
• • • • •
• • • • •
• • x • •
• • • • •
• • • • •
```

## 2.2 Image Coordinate System

### Standard Coordinate Conventions

**Matrix Indexing:**
```
Image as Matrix f(i,j):
  j→ 0  1  2  3  (columns)
i 0 [f(0,0) f(0,1) f(0,2) f(0,3)]
↓ 1 [f(1,0) f(1,1) f(1,2) f(1,3)]
  2 [f(2,0) f(2,1) f(2,2) f(2,3)]
(rows)

Convention: f(row, column) = f(i,j)
```

**Cartesian Coordinate System:**
```
y ↑
  │ 3 • • • •
  │ 2 • • • •  
  │ 1 • • • •
  │ 0 • • • •
  └─┼─┼─┼─┼→ x
    0 1 2 3

Convention: f(x,y) where x=horizontal, y=vertical
```

### Coordinate Transformations

**Matrix to Cartesian:**
```
Matrix (i,j) → Cartesian (x,y):
x = j
y = M - 1 - i  (M = number of rows)
```

**Example:**
```
3×4 image matrix (M=3):
Matrix position (1,2) → Cartesian position (2,1)
Verification: x = 2, y = 3-1-1 = 1
```

### Image Regions and Boundaries

**Rectangular ROI (Region of Interest):**
```
ROI defined by: [x₁,x₂] × [y₁,y₂]

Example ROI extraction:
Original 5×5:        Extract ROI [1:3, 1:3]:
[a b c d e]         [f g]
[f g h i j]    →    [k l]
[k l m n o]
[p q r s t]
[u v w x y]
```

## 2.3 Image Topology

### Connected Components

**Definition:** A connected component is a maximal set of connected pixels.

**Example - Binary Image Analysis:**
```
Binary Image (1=object, 0=background):
0 1 1 0 0
0 0 1 0 1
0 0 0 0 1  
1 1 0 1 1
0 1 0 0 0

Connected Components (8-connectivity):
Component 1: {(0,1), (0,2), (1,2)}
Component 2: {(1,4), (2,4), (3,3), (3,4)}
Component 3: {(3,0), (3,1), (4,1)}
```

### Boundaries and Borders

**4-boundary:** Pixels in object that have at least one 4-neighbor in background
**8-boundary:** Pixels in object that have at least one 8-neighbor in background

**Example:**
```
Object Region:           4-boundary marked with *:
1 1 1 0 0              * * * 0 0
1 1 1 0 0        →     * 1 * 0 0  
1 1 1 0 0              * * * 0 0
0 0 0 0 0              0 0 0 0 0
```

### Holes and Simply Connected Regions

**Hole:** Background region completely enclosed by object pixels

**Example:**
```
Region with hole:        Simply connected region:
1 1 1 1 1              1 1 1 1 1
1 0 0 0 1              1 1 1 1 1
1 0 0 0 1              1 1 1 1 1
1 0 0 0 1              1 1 1 1 1
1 1 1 1 1              1 1 1 1 1
  ^hole                 ^no holes
```

### Euler Number

**Definition:** χ = C - H
Where:
- χ = Euler number
- C = Number of connected components
- H = Number of holes

**Examples:**
```
Image 1: Single object, no hole    → χ = 1 - 0 = 1
Image 2: Single object, one hole   → χ = 1 - 1 = 0  
Image 3: Two objects, no holes     → χ = 2 - 0 = 2
Image 4: Two objects, each 1 hole  → χ = 2 - 2 = 0
```

---

# Chapter 3: Image Operations and Transformations

## 3.1 Arithmetic Operations

### Point-wise Addition

**Mathematical Definition:**
```
g(x,y) = f₁(x,y) + f₂(x,y)
```

**Saturation Arithmetic:** Clip results to valid range [0, L-1]
```
g(x,y) = min(L-1, max(0, f₁(x,y) + f₂(x,y)))
```

**Example - Image Averaging for Noise Reduction:**
```
Image 1:          Image 2:          Average:
[100 120 110]    [95  125 115]     [97.5 122.5 112.5]
[105 115 125] +  [110 105 120]  =  [107.5 110 122.5]
[95  130 140]    [100 135 135]     [97.5 132.5 137.5]

After rounding and clipping to [0,255]:
[98  123 113]
[108 110 123]  
[98  133 138]
```

**Application - Multiple Image Averaging:**
```
For K noisy images: g(x,y) = (1/K) Σ fₖ(x,y)
                                    k=1

Noise reduction: σ_g = σ_f / √K
Where σ_f is noise standard deviation in individual image
```

### Point-wise Subtraction

**Mathematical Definition:**
```
g(x,y) = f₁(x,y) - f₂(x,y)
```

**Example - Motion Detection:**
```
Background:       Current Frame:    Difference:
[100 105 110]    [100 105 110]     [0   0   0  ]
[115 120 125] -  [115 180 125]  =  [0   60  0  ]
[130 135 140]    [130 135 140]     [0   0   0  ]

Motion detected at position (1,1)
```

**Application - Change Detection:**
```
Change Map = |f₁(x,y) - f₂(x,y)| > threshold

Example with threshold = 30:
Difference: [5, 60, 15] → Binary: [0, 1, 0]
```

### Point-wise Multiplication

**Mathematical Definition:**
```
g(x,y) = f₁(x,y) × f₂(x,y)
```

**Example - Image Masking:**
```
Original Image:   Mask:            Masked Result:
[200 150 100]    [1 1 0]          [200 150 0  ]
[180 120 90 ] ×  [1 0 0]       =  [180 0   0  ]
[160 110 80 ]    [1 0 0]          [160 0   0  ]

Mask value 1 = keep pixel, 0 = suppress pixel
```

**ROI Selection Example:**
```
Create circular mask for center region:
Mask generation: distance from center < radius → 1, else → 0

5×5 image, center at (2,2), radius = 1.5:
[0 0 0 0 0]
[0 1 1 1 0]
[0 1 1 1 0]  
[0 1 1 1 0]
[0 0 0 0 0]
```

### Point-wise Division

**Mathematical Definition:**
```
g(x,y) = f₁(x,y) / f₂(x,y)  where f₂(x,y) ≠ 0
```

**Example - Flat-field Correction:**
```
Raw Image:        Illumination:     Corrected:
[80  100 120]    [0.8 1.0 1.2]     [100 100 100]
[90  110 130] /  [0.9 1.1 1.3]  =  [100 100 100]
[100 120 140]    [1.0 1.2 1.4]     [100 100 100]

Result shows uniform object reflectance
```

**Shading Correction Pipeline:**
```
1. Capture flat-field image (uniform illumination)
2. Normalize: flat-field / mean(flat-field)  
3. Correct image: original / normalized_flat-field
```

## 3.2 Logical Operations

### Binary Operations

**AND Operation:**
```
g(x,y) = f₁(x,y) ∧ f₂(x,y)

Truth Table:
f₁ | f₂ | g
0  | 0  | 0
0  | 1  | 0  
1  | 0  | 0
1  | 1  | 1
```

**Example - Intersection of Regions:**
```
Image 1:          Image 2:          AND Result:
[1 1 0 0]        [1 0 1 1]         [1 0 0 0]
[1 1 0 0]   AND  [1 0 1 1]    =    [1 0 0 0]
[0 0 1 1]        [0 1 1 1]         [0 0 1 1]
[0 0 1 1]        [0 1 1 1]         [0 0 1 1]
```

**OR Operation:**
```
g(x,y) = f₁(x,y) ∨ f₂(x,y)

Example - Union of Regions:
Image 1:          Image 2:          OR Result:  
[1 1 0 0]        [1 0 1 1]         [1 1 1 1]
[1 1 0 0]   OR   [1 0 1 1]    =    [1 1 1 1]
[0 0 1 1]        [0 1 1 1]         [0 1 1 1]
[0 0 1 1]        [0 1 1 1]         [0 1 1 1]
```

**NOT Operation (Complement):**
```
g(x,y) = ¬f(x,y) = 1 - f(x,y)

Example:
Original:         Complement:
[1 1 0 0]        [0 0 1 1]
[1 0 0 1]   →    [0 1 1 0]
[0 0 1 1]        [1 1 0 0]
```

**XOR Operation:**
```
g(x,y) = f₁(x,y) ⊕ f₂(x,y)

Example - Symmetric Difference:
Image 1:          Image 2:          XOR Result:
[1 1 0 0]        [1 0 1 1]         [0 1 1 1]
[1 1 0 0]  XOR   [1 0 1 1]    =    [0 1 1 1]
[0 0 1 1]        [0 1 1 1]         [0 1 0 0]
[0 0 1 1]        [0 1 1 1]         [0 1 0 0]
```

### Applications of Logical Operations

**1. Region of Interest (ROI) Extraction:**
```
ROI = Original_Image AND Mask
Where mask = 1 for ROI, 0 elsewhere
```

**2. Image Composition:**
```
Composite = (Image1 AND Mask1) OR (Image2 AND Mask2)
Where Mask1 and Mask2 are complementary
```

**3. Morphological Preprocessing:**
```
Edge_pixels = Original XOR Eroded_image
```

## 3.3 Set Operations and Statistical Operations

### Set Operations on Binary Images

**Union (∪):**
```
A ∪ B = {(x,y) | (x,y) ∈ A or (x,y) ∈ B}

Example:
Set A: {(0,0), (0,1), (1,0)}
Set B: {(0,1), (1,1), (2,0)}
A ∪ B: {(0,0), (0,1), (1,0), (1,1), (2,0)}
```

**Intersection (∩):**
```
A ∩ B = {(x,y) | (x,y) ∈ A and (x,y) ∈ B}

Using same sets:
A ∩ B: {(0,1)}
```

**Difference (-):**
```
A - B = {(x,y) | (x,y) ∈ A and (x,y) ∉ B}

A - B: {(0,0), (1,0)}
B - A: {(1,1), (2,0)}
```

**Complement (c):**
```
Aᶜ = {(x,y) | (x,y) ∉ A} within image domain

For 3×3 image, if A = {(0,0), (1,1), (2,2)}:
Aᶜ = {(0,1), (0,2), (1,0), (1,2), (2,0), (2,1)}
```

### Statistical Operations

#### First-Order Statistics

**Mean (μ):**
```
μ = (1/MN) Σ Σ f(i,j)
          i=0 j=0

Example calculation:
Image: [10 20 30]    μ = (10+20+30+40+50+60)/6 = 210/6 = 35
       [40 50 60]
```

**Variance (σ²):**
```
σ² = (1/MN) Σ Σ [f(i,j) - μ]²
           i=0 j=0

Continuing example:
Deviations²: [(10-35)², (20-35)², (30-35)², (40-35)², (50-35)², (60-35)²]
           = [625, 225, 25, 25, 225, 625]
σ² = (625+225+25+25+225+625)/6 = 1750/6 ≈ 291.67
```

**Standard Deviation (σ):**
```
σ = √σ² = √291.67 ≈ 17.08
```

**Median:**
```
Median = middle value in sorted pixel list

Example: [10, 20, 30, 40, 50, 60] → Median = (30+40)/2 = 35
```

**Mode:**
```
Mode = most frequently occurring value

Example histogram:
Value:     [10, 20, 30, 40, 50]  
Frequency: [2,  1,  5,  1,  3]  → Mode = 30
```

#### Second-Order Statistics

**Covariance:**
```
Cov(f₁,f₂) = (1/MN) Σ Σ [f₁(i,j)-μ₁][f₂(i,j)-μ₂]
                   i=0 j=0
```

**Correlation Coefficient:**
```
ρ = Cov(f₁,f₂)/(σ₁σ₂)
Range: -1 ≤ ρ ≤ 1
```

#### Local Statistical Operations

**Local Mean (Moving Average):**
```
μ(x,y) = (1/|W|) Σ f(s,t)
                 (s,t)∈W

Where W is neighborhood window around (x,y)
```

**Example - 3×3 Local Mean:**
```
Original:          Local means:
[10 20 30 40]     [15 20 25 35]
[20 30 40 50]  →  [25 30 35 45]  
[30 40 50 60]     [35 40 45 55]
[40 50 60 70]     [45 50 55 65]

Center pixel calculation:
μ(1,1) = (10+20+30+20+30+40+30+40+50)/9 = 270/9 = 30
```

## 3.4 Convolution and Correlation Operations

### Convolution Operation

**Mathematical Definition:**
```
(f * h)(x,y) = Σ  Σ  f(m,n) h(x-m, y-n)
              m=-∞ n=-∞

For discrete finite images:
g(x,y) = Σ  Σ  f(m,n) h(x-m, y-n)
         m  n
```

**Key Properties:**
1. **Commutative:** f * h = h * f
2. **Associative:** (f * g) * h = f * (g * h)
3. **Distributive:** f * (g + h) = f * g + f * h
4. **Shift Invariant:** If g = f * h, then g(x-x₀, y-y₀) = f(x-x₀, y-y₀) * h

### Convolution Step-by-Step Example

**Example Setup:**
```
Image f:           Kernel h:
[1 2 3]           [1  0]
[4 5 6]           [0 -1]  
[7 8 9]

Kernel flipped (for convolution):
[-1  0]
[ 0  1]
```

**Convolution Process:**
```
Position (1,1) calculation:
Overlay flipped kernel on image at (1,1):

Image region:     Flipped kernel:    Products:
[1 2]            [-1  0]           [-1  0]
[4 5]        ×   [ 0  1]       =   [ 0  5]

g(1,1) = -1×1 + 0×2 + 0×4 + 1×5 = 4
```

**Complete Result:**
```
Full convolution output g = f * h:
[4  2  -3]
[3  0  -6]  
[0 -5  -9]
```

### Correlation Operation

**Mathematical Definition:**
```
(f ⊙ h)(x,y) = Σ  Σ  f(m,n) h(x+m, y+n)
              m=-∞ n=-∞
```

**Relationship to Convolution:**
```
Correlation: f ⊙ h = f * h̄
Where h̄ is h rotated by 180°
```

### Template Matching Example

**Template Matching using Correlation:**
```