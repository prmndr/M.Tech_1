# Octave Digital Image Processing Commands - Complete Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Package Installation](#package-installation)
3. [Image I/O Operations](#image-io-operations)
4. [Image Properties and Conversion](#image-properties-and-conversion)
5. [Basic Image Operations](#basic-image-operations)
6. [Filtering and Enhancement](#filtering-and-enhancement)
7. [Morphological Operations](#morphological-operations)
8. [Histogram Operations](#histogram-operations)
9. [Frequency Domain Processing](#frequency-domain-processing)
10. [Noise and Restoration](#noise-and-restoration)
11. [Thresholding and Segmentation](#thresholding-and-segmentation)
12. [Color Space Operations](#color-space-operations)
13. [Geometric Transformations](#geometric-transformations)
14. [Advanced Techniques](#advanced-techniques)

---

## Introduction

Octave is a powerful open-source numerical computing environment that provides extensive capabilities for digital image processing. This guide covers the essential commands and their detailed functionality for processing digital images in Octave.

## Package Installation

### Installing Required Packages
```octave
pkg install -forge image          % Install image package from Octave Forge
pkg load image                    % Load image package into current session
pkg list                          % List all installed packages
```

**Explanation:**
- The `image` package contains most image processing functions
- Must be installed before using advanced image processing commands
- Loading the package makes functions available in the current session

---

## Image I/O Operations

### Reading Images
```octave
I = imread('image.jpg');          % Read image from file
[I, map] = imread('image.gif');   % Read indexed image with colormap
I = imread('image.png', 'png');   % Specify format explicitly
```

**Functionality:**
- `imread()` loads images from various formats (JPEG, PNG, GIF, BMP, TIFF)
- Returns image data as a matrix (2D for grayscale, 3D for color)
- For indexed images, also returns the colormap
- Automatically detects format from file extension

### Writing Images
```octave
imwrite(I, 'output.png');         % Write image to file
imwrite(I, 'output.jpg', 'quality', 95);  % JPEG with quality setting
imwrite(I, map, 'output.gif');    % Write indexed image with colormap
```

**Functionality:**
- `imwrite()` saves image matrices to files
- Format determined by file extension
- Quality parameter available for JPEG (0-100)
- Can specify additional parameters like compression

### Displaying Images
```octave
imshow(I);                        % Display image in figure window
imshow(I, []);                    % Auto-scale intensity values
figure; imagesc(I);               % Display with color scaling
colormap(gray);                   % Set colormap for display
```

**Functionality:**
- `imshow()` displays images with proper aspect ratio and scaling
- `imagesc()` scales pixel values to full colormap range
- Empty brackets `[]` enable automatic intensity scaling
- `colormap()` controls color representation for grayscale images

---

## Image Properties and Conversion

### Getting Image Information
```octave
[rows, cols, channels] = size(I); % Get image dimensions
whos I                            % Detailed variable information
class(I)                          % Data type of image
```

**Functionality:**
- `size()` returns image dimensions: height, width, and color channels
- `whos` provides memory usage and data type information
- `class()` shows the data type (uint8, double, etc.)

### Data Type Conversion
```octave
I_double = im2double(I);          % Convert to double precision [0,1]
I_uint8 = im2uint8(I);            % Convert to 8-bit integers [0,255]
I_uint16 = im2uint16(I);          % Convert to 16-bit integers
I_single = im2single(I);          % Convert to single precision
```

**Functionality:**
- `im2double()` converts to double with range [0,1] for computation
- `im2uint8()` converts to 8-bit integers with range [0,255] for storage
- `im2uint16()` provides higher precision with range [0,65535]
- Proper scaling is automatically handled during conversion

### Color Space Conversion
```octave
I_gray = rgb2gray(I);            % Convert RGB to grayscale
I_rgb = cat(3, I_gray, I_gray, I_gray); % Convert grayscale to RGB
```

**Functionality:**
- `rgb2gray()` converts color images to grayscale using luminance weights
- Formula: 0.2989*R + 0.5870*G + 0.1140*B
- `cat(3,...)` concatenates matrices along third dimension for RGB conversion

---

## Basic Image Operations

### Intensity Adjustments
```octave
I_bright = I + 50;               % Increase brightness
I_dark = I - 30;                 % Decrease brightness  
I_contrast = I * 1.5;            % Increase contrast
I_low_contrast = I * 0.7;        % Decrease contrast
I_combined = (I + 20) * 1.2;     % Combined brightness and contrast
```

**Functionality:**
- Addition/subtraction adjusts brightness (shifts intensity values)
- Multiplication/division adjusts contrast (scales intensity range)
- Operations are performed element-wise on pixel values
- Results may need clipping to valid range [0,255] for uint8

### Image Arithmetic
```octave
I_neg = 255 - I;                 % Image negative (for uint8)
I_neg_double = 1 - I;            % Image negative (for double)
I_diff = abs(I1 - I2);           % Absolute difference between images
I_avg = (I1 + I2) / 2;           % Average of two images
```

**Functionality:**
- Negative operation inverts intensity values
- Absolute difference useful for change detection
- Image averaging reduces noise through redundancy
- Operations require images of same size and type

### Geometric Operations
```octave
I_rot90 = rot90(I);              % Rotate 90 degrees counterclockwise
I_rot = imrotate(I, 45);         % Rotate by arbitrary angle
I_flip_ud = flipud(I);           % Flip upside down
I_flip_lr = fliplr(I);           % Flip left-right
I_resize = imresize(I, 0.5);     % Resize to 50% of original
I_resize_abs = imresize(I, [200 300]); % Resize to specific dimensions
```

**Functionality:**
- `rot90()` performs fast 90-degree rotations without interpolation
- `imrotate()` handles arbitrary angles using interpolation
- `flipud()/fliplr()` mirror images vertically/horizontally
- `imresize()` scales images using interpolation (default: bicubic)

### Image Cropping
```octave
I_crop = I(100:300, 50:250);     % Crop rectangular region
I_crop_all = I(100:300, 50:250, :); % Crop all color channels
[I_crop, rect] = imcrop(I);      % Interactive cropping
```

**Functionality:**
- Matrix indexing crops rectangular regions
- Colon `:` selects all elements in that dimension
- `imcrop()` provides GUI for interactive selection
- Returns both cropped image and rectangle coordinates

---

## Filtering and Enhancement

### Creating Filter Kernels
```octave
h_avg = fspecial('average', [3 3]);     % 3x3 averaging filter
h_gauss = fspecial('gaussian', [5 5], 1); % Gaussian filter, sigma=1
h_laplacian = fspecial('laplacian');    % Laplacian filter
h_sobel_x = fspecial('sobel');          % Sobel horizontal edge filter
h_unsharp = fspecial('unsharp');        % Unsharp masking filter
```

**Functionality:**
- `fspecial()` creates predefined filter kernels
- Averaging filters smooth images by local averaging
- Gaussian filters provide weighted smoothing with less edge blur
- Laplacian filters detect edges and enhance details
- Sobel filters detect horizontal or vertical edges

### Applying Filters
```octave
I_filtered = imfilter(I, h);            % Apply filter with default padding
I_filtered = imfilter(I, h, 'same');    % Keep same size as input
I_filtered = imfilter(I, h, 'replicate'); % Replicate border pixels
I_filtered = imfilter(I, h, 'circular'); % Circular boundary conditions
```

**Functionality:**
- `imfilter()` performs 2D convolution with filter kernels
- Padding options handle borders: 'same', 'replicate', 'circular', 'symmetric'
- Convolution slides filter over image computing weighted sums
- Different boundary conditions affect edge pixel processing

### Edge Detection
```octave
I_sobel = edge(I_gray, 'sobel');        % Sobel edge detection
I_canny = edge(I_gray, 'canny');        % Canny edge detection  
I_prewitt = edge(I_gray, 'prewitt');    % Prewitt edge detection
I_roberts = edge(I_gray, 'roberts');    % Roberts cross-gradient
[I_canny, thresh] = edge(I_gray, 'canny', []); % Return threshold used
```

**Functionality:**
- `edge()` detects edges using various algorithms
- Sobel uses gradient magnitude with 3x3 kernels
- Canny provides optimal edge detection with hysteresis thresholding
- Prewitt similar to Sobel but different kernel weights
- Roberts uses 2x2 diagonal kernels for fast edge detection

### Custom Filtering
```octave
% Create custom kernels
sharpen_kernel = [0 -1 0; -1 5 -1; 0 -1 0];
emboss_kernel = [-1 -1 0; -1 0 1; 0 1 1];

I_sharp = imfilter(I, sharpen_kernel);   % Apply sharpening
I_emboss = imfilter(I, emboss_kernel);   % Apply emboss effect
```

**Functionality:**
- Custom kernels enable specific image effects
- Sharpening kernels enhance edges and details
- Emboss kernels create 3D relief appearance
- Kernel design determines the filtering effect

---

## Morphological Operations

### Structuring Elements
```octave
se_disk = strel('disk', 3);             % Circular structuring element
se_square = strel('square', 5);         % Square structuring element  
se_line = strel('line', 7, 0);          % Linear structuring element
se_diamond = strel('diamond', 2);       % Diamond shape
se_custom = strel([1 1 0; 1 1 0; 0 0 0]); % Custom shape
```

**Functionality:**
- `strel()` creates structuring elements for morphological operations
- Shape and size determine the operation's effect
- Disk elements for isotropic operations
- Custom elements for specific directional processing

### Basic Morphological Operations
```octave
I_erode = imerode(I, se);               % Erosion - shrinks objects
I_dilate = imdilate(I, se);             % Dilation - expands objects
I_open = imopen(I, se);                 % Opening - erosion followed by dilation
I_close = imclose(I, se);               % Closing - dilation followed by erosion
```

**Functionality:**
- Erosion removes pixels from object boundaries (shrinks objects)
- Dilation adds pixels to object boundaries (expands objects)
- Opening removes small objects and smooths boundaries
- Closing fills small holes and connects nearby objects

### Advanced Morphological Operations
```octave
I_tophat = imtophat(I, se);             % Top-hat transform
I_bothat = imbothat(I, se);             % Bottom-hat transform
I_grad = imgradient(I, se);             % Morphological gradient
I_skel = bwmorph(I, 'skel', Inf);       % Skeletonization
```

**Functionality:**
- Top-hat highlights small bright features
- Bottom-hat highlights small dark features  
- Morphological gradient finds object boundaries
- Skeletonization reduces objects to thin connected lines

---

## Histogram Operations

### Computing Histograms
```octave
[counts, bins] = imhist(I_gray);        % Compute histogram
[counts, bins] = imhist(I_gray, 256);   % Specify number of bins
imhist(I_gray);                         % Display histogram directly
```

**Functionality:**
- `imhist()` computes intensity distribution
- Returns bin counts and bin center values
- Default uses 256 bins for 8-bit images
- Histograms show image contrast and brightness characteristics

### Histogram Enhancement
```octave
I_eq = histeq(I_gray);                  % Histogram equalization
I_eq_n = histeq(I_gray, 128);          % Equalize to n levels
I_match = histeq(I_gray, target_hist); % Match to target histogram
I_adapt = adapthisteq(I_gray);          % Adaptive histogram equalization (CLAHE)
```

**Functionality:**
- Histogram equalization spreads intensity values uniformly
- Improves contrast in images with poor dynamic range
- Adaptive methods (CLAHE) prevent over-amplification of noise
- Histogram matching transforms one distribution to match another

### Histogram Analysis
```octave
mean_intensity = mean(I_gray(:));       % Mean intensity
std_intensity = std(double(I_gray(:))); % Standard deviation
[min_val, max_val] = bounds(I_gray(:)); % Min and max values
entropy_val = entropy(I_gray);          % Image entropy
```

**Functionality:**
- Statistical measures characterize image properties
- Mean indicates average brightness
- Standard deviation measures contrast
- Entropy quantifies information content

---

## Frequency Domain Processing

### Fourier Transform
```octave
F = fft2(double(I_gray));               % 2D FFT
F_shifted = fftshift(F);                % Shift zero frequency to center
magnitude = abs(F_shifted);             % Magnitude spectrum
phase = angle(F_shifted);               % Phase spectrum
log_mag = log(1 + magnitude);           % Log magnitude for display
```

**Functionality:**
- `fft2()` computes 2D Fast Fourier Transform
- Converts image from spatial to frequency domain
- `fftshift()` moves DC component to center for visualization
- Magnitude spectrum shows frequency content strength
- Phase spectrum contains spatial information

### Inverse Transform
```octave
I_reconstructed = real(ifft2(F));       % Inverse FFT
I_from_mag = real(ifft2(ifftshift(magnitude))); % From magnitude only
```

**Functionality:**
- `ifft2()` converts back to spatial domain
- `real()` extracts real part (imaginary part should be near zero)
- Reconstruction from magnitude only loses phase information

### Frequency Domain Filtering
```octave
% Create frequency domain filters
[M, N] = size(I_gray);
[u, v] = meshgrid(1:N, 1:M);
D = sqrt((u - N/2).^2 + (v - M/2).^2); % Distance from center

% Low-pass filter (smoothing)
cutoff = 30;
H_lp = double(D <= cutoff);
F_filtered = F_shifted .* H_lp;
I_lowpass = real(ifft2(ifftshift(F_filtered)));

% High-pass filter (edge enhancement)
H_hp = double(D > cutoff);  
F_filtered = F_shifted .* H_hp;
I_highpass = real(ifft2(ifftshift(F_filtered)));
```

**Functionality:**
- Frequency domain filtering multiplies transform with filter
- Low-pass filters remove high frequencies (smoothing effect)
- High-pass filters remove low frequencies (edge enhancement)
- Circular filters provide isotropic frequency response

---

## Noise and Restoration

### Adding Noise
```octave
I_gauss = imnoise(I_gray, 'gaussian', 0, 0.01);     % Gaussian noise
I_salt = imnoise(I_gray, 'salt & pepper', 0.05);    % Salt & pepper noise
I_poisson = imnoise(I_gray, 'poisson');             % Poisson noise
I_speckle = imnoise(I_gray, 'speckle', 0.04);       % Speckle noise
```

**Functionality:**
- `imnoise()` adds various types of noise for testing
- Gaussian noise: additive with specified mean and variance
- Salt & pepper: random black and white pixels
- Poisson noise: signal-dependent noise model
- Speckle: multiplicative noise common in radar/ultrasound

### Noise Reduction Filters
```octave
% Linear filters
I_avg = imfilter(I_noisy, fspecial('average', [3 3])); % Average filter
I_gauss = imfilter(I_noisy, fspecial('gaussian', [5 5], 1)); % Gaussian filter

% Nonlinear filters  
I_median = medfilt2(I_noisy, [3 3]);              % Median filter
I_wiener = wiener2(I_noisy, [5 5]);               % Wiener filter
```

**Functionality:**
- Average filters reduce Gaussian noise but blur edges
- Gaussian filters provide weighted averaging with better edge preservation
- Median filters effectively remove salt & pepper noise
- Wiener filters adapt to local image statistics for optimal restoration

### Advanced Restoration
```octave
% Estimate noise parameters
noise_var = mean((I_noisy(:) - I_original(:)).^2); % Noise variance estimate

% Adaptive filtering
I_adaptive = wiener2(I_noisy, [5 5], noise_var);  % Wiener with known noise

% Morphological noise removal (for binary images)
se = strel('disk', 1);
I_clean = imclose(imopen(I_binary_noisy, se), se); % Open-close filtering
```

**Functionality:**
- Noise estimation helps optimize filter parameters
- Adaptive filters adjust based on local image content
- Morphological filtering removes noise while preserving shape

---

## Thresholding and Segmentation

### Global Thresholding
```octave
threshold = graythresh(I_gray);                    % Otsu's method
I_binary = imbinarize(I_gray, threshold);          % Apply threshold
I_binary = I_gray > threshold;                     % Manual thresholding
I_multi = imquantize(I_gray, [0.3 0.6]);          % Multi-level thresholding
```

**Functionality:**
- `graythresh()` finds optimal threshold using Otsu's method
- Otsu minimizes intra-class variance to separate foreground/background
- `imbinarize()` converts to binary image using threshold
- Multi-level creates multiple intensity regions

### Adaptive Thresholding
```octave
I_adaptive = adaptthresh(I_gray, 0.4);             % Adaptive threshold map
I_binary = imbinarize(I_gray, I_adaptive);         % Apply adaptive threshold
I_local = imbinarize(I_gray, 'adaptive', 'Sensitivity', 0.6); % Local adaptive
```

**Functionality:**
- Adaptive thresholding handles varying illumination
- Local thresholds computed for image neighborhoods  
- Sensitivity parameter controls threshold adaptation
- Better than global methods for uneven lighting

### Region-Based Segmentation
```octave
% Connected component labeling
[L, num_regions] = bwlabel(I_binary);              % Label connected components
stats = regionprops(L, 'Area', 'Centroid');       % Measure region properties
areas = [stats.Area];                             % Extract areas
large_regions = areas > 100;                      % Filter by size
```

**Functionality:**
- `bwlabel()` assigns unique labels to connected regions
- `regionprops()` measures geometric properties of regions
- Common properties: area, centroid, bounding box, perimeter
- Enables object counting and size filtering

---

## Color Space Operations

### RGB to Other Color Spaces
```octave
I_hsv = rgb2hsv(I);              % RGB to HSV (Hue, Saturation, Value)
I_lab = rgb2lab(I);              % RGB to LAB (perceptually uniform)
I_xyz = rgb2xyz(I);              % RGB to XYZ (CIE standard)
I_ycbcr = rgb2ycbcr(I);          % RGB to YCbCr (video standard)
```

**Functionality:**
- HSV separates color (hue) from intensity (value)
- LAB provides perceptually uniform color space
- XYZ is device-independent color representation
- YCbCr separates luminance from chrominance

### Working with Color Channels
```octave
% Split color channels
R = I(:,:,1);                    % Red channel
G = I(:,:,2);                    % Green channel  
B = I(:,:,3);                    % Blue channel

% Recombine channels
I_modified = cat(3, R*1.2, G, B*0.8); % Enhance red, reduce blue

% HSV manipulation
I_hsv = rgb2hsv(I);
I_hsv(:,:,2) = I_hsv(:,:,2) * 1.5;    % Increase saturation
I_enhanced = hsv2rgb(I_hsv);           % Convert back to RGB
```

**Functionality:**
- Channel separation enables independent processing
- `cat(3,...)` concatenates channels back to color image
- HSV manipulation allows intuitive color adjustments
- Saturation enhancement makes colors more vivid

### Color-Based Segmentation
```octave
I_hsv = rgb2hsv(I);
hue = I_hsv(:,:,1);
sat = I_hsv(:,:,2);

% Create color mask
blue_mask = (hue > 0.5) & (hue < 0.8) & (sat > 0.3);
I_blue_objects = I;
I_blue_objects(repmat(~blue_mask, [1 1 3])) = 0;  % Keep only blue regions
```

**Functionality:**
- Color segmentation isolates objects by color
- HSV space often better than RGB for color-based selection
- Logical operations create binary masks
- `repmat()` replicates mask for all color channels

---

## Geometric Transformations

### Affine Transformations
```octave
% Translation
tform_translate = affine2d([1 0 0; 0 1 0; 50 30 1]);

% Rotation  
theta = pi/4;  % 45 degrees
tform_rotate = affine2d([cos(theta) -sin(theta) 0; sin(theta) cos(theta) 0; 0 0 1]);

% Scaling
tform_scale = affine2d([2 0 0; 0 2 0; 0 0 1]);

% Apply transformation
I_transformed = imwarp(I, tform_translate);
```

**Functionality:**
- Affine transformations preserve parallel lines
- Include translation, rotation, scaling, shearing
- 3x3 transformation matrix defines the mapping
- `imwarp()` applies transformation with interpolation

### Perspective Transformation
```octave
% Define corresponding points
fixed_points = [0 0; 100 0; 100 100; 0 100];      % Square corners
moving_points = [10 20; 80 5; 120 90; 5 110];     % Distorted corners

% Create perspective transformation
tform_perspective = fitgeotrans(moving_points, fixed_points, 'projective');
I_corrected = imwarp(I, tform_perspective);
```

**Functionality:**
- Perspective transforms correct keystone distortion
- `fitgeotrans()` computes transformation from point correspondences
- Requires at least 4 point pairs for perspective transformation
- Useful for document scanning and camera calibration

### Image Registration
```octave
% Template matching for alignment
template = I1(100:150, 100:150);        % Extract template
correlation = normxcorr2(template, I2);  % Normalized cross-correlation
[max_corr, max_idx] = max(correlation(:)); % Find best match location
[y_peak, x_peak] = ind2sub(size(correlation), max_idx);
```

**Functionality:**
- `normxcorr2()` finds template matches in images
- Returns correlation values from -1 to 1
- Peak location indicates best template match
- Used for object tracking and image alignment

---

## Advanced Techniques

### Watershed Segmentation
```octave
% Prepare image for watershed
I_complement = imcomplement(I_gray);     % Complement for watershed
I_dist = bwdist(~I_binary);             % Distance transform
I_watershed = watershed(I_dist);         % Apply watershed
I_segmented = I_binary;
I_segmented(I_watershed == 0) = 0;      % Mark boundaries
```

**Functionality:**
- Watershed treats image as topographic surface
- Finds catchment basins separated by watershed lines
- Distance transform creates peaks at object centers
- Effective for separating touching objects

### Feature Detection
```octave
corners = corner(I_gray, 'Harris');      % Harris corner detection  
corners = corner(I_gray, 'FAST');        % FAST corner detection
[features, valid_points] = extractFeatures(I_gray, corners); % Extract descriptors
```

**Functionality:**
- Corner detection finds interest points for tracking
- Harris detector based on gradient structure tensor
- FAST detector uses intensity comparisons around point
- Feature descriptors enable point matching between images

### Image Quality Metrics
```octave
mse_value = immse(I1, I2);               % Mean squared error
psnr_value = psnr(I1, I2);               % Peak signal-to-noise ratio  
ssim_value = ssim(I1, I2);               % Structural similarity index
```

**Functionality:**
- MSE measures average pixel differences squared
- PSNR relates to reconstruction quality (higher is better)
- SSIM considers structural information and human perception
- Used to evaluate image processing algorithm performance

---

## Practical Examples and Applications

### Example 1: Image Enhancement Pipeline
```octave
% Load and prepare image
I = imread('input.jpg');
I_gray = rgb2gray(I);

% Noise reduction
I_smooth = imfilter(I_gray, fspecial('gaussian', [3 3], 0.8));

% Contrast enhancement
I_enhanced = histeq(I_smooth);

% Sharpening
sharpen_kernel = [0 -0.5 0; -0.5 3 -0.5; 0 -0.5 0];
I_sharp = imfilter(I_enhanced, sharpen_kernel);

% Display results
figure;
subplot(2,2,1); imshow(I_gray); title('Original');
subplot(2,2,2); imshow(I_smooth); title('Smoothed');  
subplot(2,2,3); imshow(I_enhanced); title('Enhanced');
subplot(2,2,4); imshow(I_sharp); title('Sharpened');
```

### Example 2: Object Detection and Measurement
```octave
% Load and threshold image
I = imread('objects.jpg');
I_gray = rgb2gray(I);
I_binary = imbinarize(I_gray, graythresh(I_gray));

% Clean up binary image
se = strel('disk', 2);
I_clean = imopen(I_binary, se);

% Label and measure objects
[L, num_objects] = bwlabel(I_clean);
stats = regionprops(L, 'Area', 'Centroid', 'BoundingBox');

% Filter objects by size
min_area = 100;
valid_objects = [stats.Area] > min_area;
fprintf('Found %d objects larger than %d pixels\n', sum(valid_objects), min_area);

% Visualize results
figure;
imshow(I); hold on;
for i = find(valid_objects)
    rectangle('Position', stats(i).BoundingBox, 'EdgeColor', 'r');
    plot(stats(i).Centroid(1), stats(i).Centroid(2), 'r+', 'MarkerSize', 10);
end
```

---

## Tips and Best Practices

### Memory Management
- Use appropriate data types: uint8 for storage, double for computation
- Process images in blocks for large datasets
- Clear unnecessary variables with `clear variable_name`
- Monitor memory usage with `whos` command

### Performance Optimization  
- Vectorize operations instead of loops when possible
- Use built-in functions rather than implementing from scratch
- Consider image size for real-time applications
- Pre-allocate arrays for better performance

### Error Handling
```octave
try
    I = imread('image.jpg');
catch
    error('Could not load image file');
end

% Check image properties
if ndims(I) == 3
    I_gray = rgb2gray(I);
else
    I_gray = I;
end
```

### Quality Considerations
- Always validate input parameters
- Check for edge cases (empty images, single pixel images)
- Consider boundary conditions for filtering operations
- Test with various image types and sizes

---

## Conclusion

This comprehensive guide covers the essential Octave commands for digital image processing. The combination of basic operations, advanced techniques, and practical examples provides a solid foundation for developing image processing applications. Remember to experiment with parameters and combine techniques to achieve desired results.

For additional functionality, explore specialized toolboxes and contribute to the open-source Octave community. The flexibility and power of Octave make it an excellent choice for both research and practical image processing applications.