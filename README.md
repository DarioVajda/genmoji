# **Comparing Autoregressive and Diffusion Models on Emoji Generation**



### **Dario Vajda**

### Faculty of Computer and Information Science, University of Ljubljana

vajdadario@gmail.com
# **Abstract**
Denoising Diffusion Probabilistic Models (DDPM) [1] have proven superior to earlier image generation approaches, such as Autoregressive models like PixelCNN [2]. In this research project, both methods are applied to the simple task of generating new images of emojis using a very small dataset, with the aim of highlighting their differences.

**1 Introduction**

Denoising Diffusion Probabilistic Models are developing very rapidly. However, let's take a step back and consider where they came from, how and why they were invented, and what makes them so much better than older image generation approaches, such as autoregressive models. In this project, a basic DDPM and a PixelCNN-like model will be trained on a small dataset with emojis from Kaggle [3]. The images were scaled down to 32x32 resolution for simplicity.

**2 Method**

For more consistent and coherent image generation, the dataset was filtered to include only yellow face emojis instead of the full dataset, as the models in question were quite small and unable to handle a broad range of variability in the data.

**2.1 PixelCNN**

PixelCNN is an autoregressive model used for image generation. As the name suggests, the architecture is based on Convolutional Neural Networks. Each new pixel is generated sequentially based on the previously generated pixels. It leverages masked convolutional layers to make sure that each pixel has information only on the previous pixels. Moreover, the inherent ability to parallelize CNN computations allows the PixelCNN model to train very efficiently on the GPU. Conceptually, the entire training pipeline can be seen as learning to restore lost information—that is, predicting subsequent pixels in an image. (this intuition is good to have when transitioning to Diffusion model architecture). For this project, the CNN consisted of 4 convolutional layers and the output was discretized to 4-bit RGB channels to keep the model small and easy to train.

![](https://github.com/DarioVajda/genmoji/blob/main/readme_images/pixelcnn.png)

figure 1. PixelCNN Convolutional Layer

**2.2 DDPM**

Denoising Diffusion Probabilistic Models are based on the principle of restoring lost information. In this approach, the information loss is not introduced by removing pixels, but by adding noise to the image. The model learns to predict the noise added to an image, and after training, it can generate a new, original image resembling the dataset, by repeatedly removing the predicted noise from an image initialized with random noise. This gradual denoising process enables DDPMs to capture complex data distributions, resulting in high-quality image synthesis and a robust representation of visual structures. The noise prediction model is implemented using the U-Net architecture [4], with only two downsampling and two upsampling layers and a total of 5 convolutional layers inbetween.
 
<img
  src="https://github.com/DarioVajda/genmoji/blob/main/readme_images/diffusion.png"
  height="250"
  alt="diffusion"
/>

Figure 2. Forward and Backward Diffusion Processes	       

<img
  src="https://github.com/DarioVajda/genmoji/blob/main/readme_images/unet.png"
  height="350"
  alt="unet"
/>

Figure 3. U-Net Model Architecture

**3 Experiments**

Both models were trained for the same amount of time on the same hardware for a fair comparison. One of the big differences was inference speed, where the diffusion model required less than one-tenth of the time for one new sample. This is because a pixel by pixel autoregressive model requires *O(height\*width)* time for a new sample, while a diffusion model does it in *O(T)* time, where T is the number of steps (500 in this case).

<img
  src="https://github.com/DarioVajda/genmoji/blob/main/readme_images/emoji1.png"
  width="150"
  height="150"
  alt="emoji1"
/>
<img
  src="https://github.com/DarioVajda/genmoji/blob/main/readme_images/emoji2.png"
  width="150"
  height="150"
  alt="emoji2"
/>
<img
  src="https://github.com/DarioVajda/genmoji/blob/main/readme_images/emoji3.png"
  width="150"
  height="150"
  alt="emoji3"
/>
<img
  src="https://github.com/DarioVajda/genmoji/blob/main/readme_images/emoji4.png"
  width="150"
  height="150"
  alt="emoji4"
/>

Figure 4. PixelCNN-like Model outputs

<img
  src="https://github.com/DarioVajda/genmoji/blob/main/readme_images/emoji5.png"
  width="150"
  height="150"
  alt="emoji5"
/>
<img
  src="https://github.com/DarioVajda/genmoji/blob/main/readme_images/emoji6.png"
  width="150"
  height="150"
  alt="emoji6"
/>
<img
  src="https://github.com/DarioVajda/genmoji/blob/main/readme_images/emoji7.png"
  width="150"
  height="150"
  alt="emoji7"
/>
<img
  src="https://github.com/DarioVajda/genmoji/blob/main/readme_images/emoji8.png"
  width="150"
  height="150"
  alt="emoji8"
/>

Figure 5. Diffusion Model outputs

The PixelCNN model was better at producing results with consistent colours, while on the other hand, the Diffusion model was better at producing more realistic shapes and structures of faces, but struggled a little bit with the true tones of the colours.

**4 Future Plans**

**4.1 Questionnaire**

Since image generation quality is very subjective, one way to evaluate their quality is to create a questionnaire, compile the responses and analyse the results.

**4.1 Possible improvements**

Implementing a prompting feature for both models with a text encoder would enable the models to generate images with specific learned features on demand, instead of generating images without any contextual guidance, relying solely on random seeds. Seeing how the two approaches adapt to this small-scale problem would definitely be very interesting.

