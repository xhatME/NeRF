# Neural Radience Field (NeRF)
The Nerfs project is divided into two comprehensive yet interconnected components. Part 1 concentrates on the application of a neural network model to accurately represent a 2D image. This entails the development and training of a 2D model, which learns the correlation between normalized pixel coordinates and their corresponding RGB values. Essential elements of this part include a structured 2D neural network model, a positional encoding function, and various methods for generating normalized coordinates and training the neural network.  Part 2 delves into the exploration of Neural Radiance Fields (NeRFs), which serve as a robust framework for representing and rendering 3D scenes. This segment encompasses critical functionalities such as positional encoding, ray generation, stratified sampling, the definition of the NeRF model, batch processing, and volumetric rendering. Both segments collectively enhance the field of computer vision by providing advanced tools for reconstructing both 2D and 3D scenes. This framework not only facilitates the understanding of image and scene representation through neural networks but also lays a foundation for further research and practical applications in the area.

## Part 1: Fitting a 2D Image
![Neural_Radience_Field_NeRF](2d.gif)
Part 1 of the project focuses on utilizing a neural network model to fit a 2D image. This involves training a model to learn the mapping from normalized pixel coordinates to RGB values, effectively reconstructing the target image.

### Components

#### 1. **Positional Encoding**
   - **Objective**: Enhance the model's ability to approximate high-frequency functions by mapping input coordinates into a higher-dimensional space.
   - **Method**: Utilizes a sinusoidal periodic function for positional encoding, which improves the network's performance in representing variations in color and texture.
   - **Implementation Details**: 
     - The function `positional_encoding()` takes an input vector and applies sinusoidal transformations to expand its dimensionality.
     - This encoding aids the network in handling complex patterns and details in the image.

#### 2. **MLP Design**
   - **Objective**: Define and implement a multilayer perceptron (MLP) that learns the image representation from encoded coordinates.
   - **Architecture**:
     - The MLP consists of three linear layers:
       1. The first layer maps the encoded features to an intermediate dimension.
       2. The second layer processes features at the same dimensionality.
       3. The final layer outputs the RGB values, scaled using a sigmoid activation to fit within the [0,1] range.
     - Activation Functions: ReLU activations are used in the first two layers to introduce non-linearity, improving learning capability.
   - **Functionality**: 
     - The network, once trained, can generate RGB values from any given coordinate, effectively reconstructing the original image.

#### 3. **Fitting the Image**
   - **Objective**: Train the MLP to fit the provided image by minimizing the difference between the original and reconstructed images.
   - **Process**:
     - Normalize pixel coordinates to the range [0, 1].
     - Apply positional encoding to these coordinates to prepare them for input into the MLP.
     - Train the network using a mean squared error loss function and monitor performance using the Peak Signal-to-Noise Ratio (PSNR).
   - **Results**: 
     - The model is evaluated by comparing the reconstructed image against the original, with improvements noted in the ability to capture high-frequency details as the positional encoding's frequency increases.
### Extending the Model's Application

#### 1. **Using Other Metrics**
   - Apart from PSNR, other metrics like Structural Similarity Index (SSIM) and Mean Absolute Error (MAE) can be used to provide a more comprehensive evaluation of image quality and similarity.

#### 2. **Experimenting with Different Images**
   - The flexibility of the model allows for testing with various images, including natural scenes, artworks, or synthetic patterns. This can help in understanding the model's strengths and limitations across different types of visual content.

#### 3. **Further Experiments**
   - **Frequency Analysis**: Experimenting with different numbers of frequencies in positional encoding can shed light on its impact on the network’s ability to capture fine details.
   - **Architecture Variations**: Modifying the MLP design, such as increasing the number of layers or changing activation functions, can explore how these changes affect the fidelity of the reconstructed image.

### Summary
This part of the project not only tests the efficacy of neural networks in image approximation tasks but also demonstrates the critical role of positional encoding in enhancing the model's ability to capture and recreate complex visual details. The tools and techniques developed here provide a solid foundation for advancing computer vision capabilities in both academic and practical applications.

## Part 2: Fitting a 3D Scene
![Neural_Radience_Field_NeRF](nerf.gif)

In Part 2, the project explores the representation and rendering of 3D scenes using Neural Radiance Fields (NeRF). This framework is utilized for reconstructing complex 3D environments from 2D images, offering a detailed and dynamic view synthesis.

### Components

#### 1. **Ray Generation**
   - **Objective**: Compute the origin and direction of rays for each pixel in 2D images to facilitate 3D scene reconstruction.
   - **Method**: Utilizes camera intrinsic parameters and transformation matrices to calculate how rays traverse through the 3D space.
   - **Implementation Details**:
     - The function `get_rays()` calculates the origin and direction for each ray based on the camera's position and orientation.

#### 2. **Sampling Points Along a Ray**
   - **Objective**: Sample multiple points along each ray to estimate the properties of the scene at different depths.
   - **Method**: Implements stratified sampling to distribute samples uniformly along each ray.
   - **Implementation Details**:
     - The function `stratified_sampling()` calculates evenly spaced depth points along the rays, which are used to sample the radiance and density.

#### 3. **NeRF MLP Design**
   - **Objective**: Define and implement a neural network to estimate the color and density at each sampled point in the scene.
   - **Architecture**:
     - Consists of several fully connected layers with ReLU activations, incorporating skip connections and separate pathways for color and density to reflect different dependencies on the input data.
   - **Functionality**:
     - Predicts both the RGB color and the density at each point based on its position and viewing direction, which are crucial for accurate volume rendering.

#### 4. **Volume Rendering**
   - **Objective**: Reconstruct the color of pixels by integrating the contributions of all sampled points along each ray.
   - **Method**: Uses a weighted sum of colors and densities to simulate how light interacts with the scene.
   - **Implementation Details**:
     - The function `volumetric_rendering()` computes the visible color for each pixel by considering the transmittance and absorption along the ray.

#### 5. **Rendering an Image**
   - **Objective**: Use all previously defined components to compute the color of each pixel in a rendered image from a novel view.
   - **Process**:
     - Combines ray generation, point sampling, and volume rendering in a single pipeline to reconstruct a full 2D image from the 3D scene data.

#### 6. **Integration and Training**
   - **Objective**: Optimize the neural network to improve the reconstructed image quality over multiple iterations.
   - **Method**: Uses gradient descent with the Adam optimizer, based on the mean squared error between the reconstructed and reference images.

### Extending the Model's Application

#### 1. **Using Other Metrics**
   - **Alternative Metrics**: Incorporate metrics like SSIM or MAE alongside PSNR to provide a more nuanced assessment of image quality and reconstruction accuracy.

#### 2. **Experimenting with Different Scenes**
   - **Variety of Scenes**: Test the NeRF model on different types of scenes, such as indoor environments, natural landscapes, or complex geometric structures, to evaluate its generalization capabilities.

#### 3. **Advanced Experiments**
   - **Lighting Variations**: Explore the effects of different lighting conditions on scene reconstruction to understand the model's robustness to changes in illumination.
   - **Geometry Complexity**: Assess how the complexity of the scene geometry affects the NeRF's ability to accurately render details and textures.

### Summary
Part 2 of the project pushes the boundaries of 3D scene reconstruction, demonstrating the potential of NeRF in producing photorealistic renderings from a set of 2D images. The combination of advanced neural network architectures and innovative rendering techniques enables the creation of dynamic, high-fidelity visualizations of complex environments. This segment sets a solid foundation for further exploration and practical applications in virtual reality, augmented reality, and visual effects.
