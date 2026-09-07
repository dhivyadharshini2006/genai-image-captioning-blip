## Prototype Development for Image Captioning Using the BLIP Model and Gradio Framework

### AIM:
To design and deploy a prototype application for image captioning by utilizing the BLIP image-captioning model and integrating it with the Gradio UI framework for user interaction and evaluation.

### PROBLEM STATEMENT:
Image captioning is the task of generating a textual description for a given image. This is crucial for applications like accessibility tools for visually impaired users, content generation, and image indexing. Traditional systems often rely on predefined labels or are limited in context understanding. By leveraging the BLIP model—a state-of-the-art vision-language pretraining model—this project aims to create an intuitive and efficient application for real-time image captioning, accessible via the Gradio interface.


### DESIGN STEPS:
STEP 1:
Model Preparation
Use a pre-trained BLIP image-captioning model available from Hugging Face Transformers or similar libraries. Ensure the model supports inference on diverse image types and contexts.

STEP 2:
Framework
Use Gradio to create a UI with the following components: Input: File upload for images. Output: Textbox showing the generated caption.

STEP 3:
Workflow
Load the BLIP model and tokenizer. Accept an image as input via Gradio's file upload. Preprocess the image for the BLIP model. Generate a caption using the BLIP model's inference pipeline. Display the caption on the Gradio interface.

STEP 4:
Testing and Deployment
Test the application with various image types to ensure the captions are meaningful and diverse. Deploy the application on a public URL using Gradio’s hosting features or external platforms like Hugging Face Spaces.

### PROGRAM:
```python
import os
from PIL import Image
import gradio as gr
from google import genai

# 1. Set Gemini API Key (Replace with your actual key)
api_key = "YOUR API key"
client = genai.Client(api_key=api_key)

# 2. Define Image Captioning Function
def captioner(image):
    if image is None:
        return "Please upload an image."
    try:
        prompt = "Write a short, descriptive caption for this image."
        response = client.models.generate_content(
            model='gemini-3.6-flash',
            contents=[image, prompt]
        )
        return response.text
    except Exception as e:
        return f"Error: {str(e)}"

# 3. Build & Launch Gradio Interface
gr.close_all()

demo = gr.Interface(
    fn=captioner,
    inputs=[gr.Image(label="Upload image", type="pil")],
    outputs=[gr.Textbox(label="Caption", lines=3)],
    title="Image Captioning with Gemini",
    description="Upload an image to generate a descriptive caption using Gemini 3.6 Flash.",
    flagging_mode="never"
)

# Launches inline inside your Jupyter Notebook
demo.launch(inline=True)
```




### OUTPUT:
<img width="726" height="342" alt="image" src="https://github.com/user-attachments/assets/9573c103-9393-4492-8cd8-c65d3db527bf" />

### RESULT:
The application successfully generates high-quality images based on user-provided text prompts. The Stable Diffusion model ensures visually appealing results, and the Gradio interface makes it accessible and interactive.
