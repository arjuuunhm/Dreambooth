# Implementation of Google's Dreambooth: Fine Tuning Text-to-Image Diffusion Models for Subject Driven Generation

3.1 – Introduction
Fine Tuning Text-to-Image Diffusion Models for Subject Driven Generation (Dreambooth) was published at CVPR 2023 by Nataniel Ruiz, Yuanzhen Li, Varun Jampani, Yael Pritch, Michael Rubinstein, Kfir Aberman. They are all researchers at Google and Nataniel Ruiz is also a Professor at Boston University. The motivation behind this project is that large text-to-image models lack the ability to mimic the appearance of subjects in a given reference set and synthesize novel renditions of them in different contexts.

The goal of this approach is to take a pre-trained text-to-image model (In the paper it is Imagen and in our implementation it is a Stable Diffusion model) and finetune it so that it learns to bind a certain identifier token [V] with the subject of the 3-5 images. An example of this is if I have a specific photo of my dog. Dreambooth seeks to leverage the pre-trained model’s prior understanding of what a dog is and entangle it with the embedding of my dog’s unique identifier [V]. 
![Screenshot 2024-05-18 at 8 27 19 PM](https://github.com/arjuuunhm/DreamBooth/assets/96384102/eddb2fa0-83f6-43b8-979d-769bdefa4a5b)

The main contributions that the paper introduces are the following: 
1. Rare-identifier token 
The authors found that using common words messed with the model’s prior understanding of said words, and using gibberish like “xxxyyy55ttt” lead to artifacts in images rendered by the model. They proposed using words that have low frequency in the diffusion model’s language model.
2. Prior-preservation Loss
The fine-tuning process introduces language drift. This leads to the model losing its semantic understanding for the class and overfitting. The authors introduce a prior-preservation loss as a regularization term.
The authors introduce the use of a frozen model for this. The sample images of their class (Ex. a dog) from the frozen model and attempt to recover a noised version of those samples in the fine tuned model. The fine tuned model also attempts to recover noised samples of [V] class. This encourages the fine-tuned model to learn to bind the [V] token with the subject but also not forget its understanding of the class itself.
![Screenshot 2024-05-18 at 8 26 59 PM](https://github.com/arjuuunhm/DreamBooth/assets/96384102/acc88c23-b4cc-49a9-a6e0-648a298fec39)

3. Proposed the DINO metric which measures subject fidelity of generated images

3.2 – Chosen Result
The results we attempted to replicate were the DreamBooth (Stable Diffusion) DINO results which was introduced by the group. The DINO metric is the average pairwise cosine similarity between the ViT-S/16 DINO embeddings of generated and real images.

Below is a table where you can see the results from the paper. Pay attention to the Stable Diffusion results since that is the one we aimed to replicate. This is because Imagen is not a public model.
![Screenshot 2024-05-18 at 8 27 56 PM](https://github.com/arjuuunhm/DreamBooth/assets/96384102/073e745d-f48c-403f-b518-e063c16c8531)
It is worth noting that this DINO metric is not perfect because our subject will be in different orientations and scenarios. As you can see in the diagram under real images DINO score is 0.774. We can see DreamBooth with Stable Diffusion has a score of 0.668. 

3.5 – Conclusion and Future Work
We learned a lot from working on this project. Before working on this project we knew a bit about stable diffusion but now we understand a lot more about all the components in the stable diffusion pipeline and what role each component plays. We also understand more about how image processing works in the latent space. On prior coding assignments we didn’t really run into CUDA Out of Memory errors as much before the workloads were suited towards the GPUs. Working on this project we learned a lot more about how GPU programming and memory works. We also got a better understanding of how to use open-source resources like hugging face. We got more practice evaluating our models by using metrics like DINO. We got more hands-on research experience by taking a research paper, like DreamBooth, and implementing it from scratch.  This gave us more insight into the models and architectures to consider when implementing this method, considering the little amount of implementation details we were given. We both were intrigued by lots of the fine-tuning GenAI applications that were happening in industry. We are glad we got the opportunity to experience first-hand work in this area. 
One of our biggest challenges were with the level of noise left in our outputs. We believe that we weren’t able to figure out exactly how to configure the noise-scheduler and unet timesteps when training. We were not able to find how the authors implemented it in their stable diffusion model since they spoke more about the imagen model in the paper and didn’t share their codebase. We were lucky to obtain a better GPU for our training later on as we were working on this project. We did not have this chance for the majority of our project. Some areas to explore are possibly using smaller diffusion models with fewer parameters to reduce computation overhead and memory requirements. Another strategy worth exploring is mixed precision training. We found that when using quantization with 16-bit floating point types we were able to avoid memory issues, but we had a lot of NaNs in our loss calculation so we switched back to 32-bit floating point types. By using mixed-precision we could definitely improve memory performance while having more stability. 
We could extend our model to other forms of generative AI. The main idea of Dream Booth is to take a subject and fine-tune an existing model so that the subject can be inserted into different domains. We feel this objective has the most potential and relevance in the context of generative AI. For example we have heard AI generated music of rappers like Drake. A similar fine-tuning approach on text-to-speech models can be done to bring artists into a new genre of music. This image problem can be transferred to the domain of videos as well. It seems like a similar problem since now we are just adding time as a dimension. It is worth noting though that such models can be dangerous as they could potentially be used to generate fake images of people which could be used for malicious purposes.


