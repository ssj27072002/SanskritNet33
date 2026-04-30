# SanskritNet: End-to-End Cross-Lingual Poetic Style Transfer

Link for project(If not working run again code it will appear for you again as it free for 3 days) : https://e5f531fcdd4e9f786b.gradio.live

## Project Overview
**SanskritNet** is an end-to-end deep learning web application designed to bridge the linguistic gap between classical Sanskrit texts and historical poetic dialects. It translates classical Sanskrit Shlokas (or modern Hindi text) into the poetic Awadhi style of the *Ramcharitmanas*. 

Because direct parallel translation data between Sanskrit and Awadhi is virtually non-existent, this application introduces a **novel two-stage cascaded pipeline**. It first extracts the semantic meaning and then applies a historical poetic style transfer, ensuring high accuracy and preventing model hallucination.

---

## System Architecture (Where is everything?)

Since this project is designed for seamless execution in cloud notebook environments (like Google Colab), the entire full-stack application runs cohesively from a unified Python environment. 

### 1. The Frontend (UI & User Interaction)
* **Technology:** Built using [Gradio](https://gradio.app/).
* **Location:** Integrated directly within the main script (under the `GRADIO FRONTEND` section).
* **How it works:** The frontend generates an interactive, bilingual web interface (HTML/CSS/JS) entirely via Python. It captures user inputs (Sanskrit/Hindi), passes them to the backend, and dynamically renders the translated outputs, English glosses, and system confidence scores. When executed, it provisions a public `gradio.live` URL for immediate global access.

### 2. The Backend (Pipeline Logic & Data Flow)
* **Technology:** Python, PyTorch, Deep-Translator API.
* **Location:** Housed in the `BACKEND LOGIC` section of the main script.
* **How it works:** The backend manages the two-stage pipeline:
  * **Stage 1 (Semantic Bridge):** Routes the Sanskrit input to the `deep-translator` Google Translate API to convert it into literal, modern Hindi.
  * **Stage 2 (Inference):** Passes the modern Hindi string into the deep learning model.
  * **Safety Mechanisms:** The backend includes a custom Python cleaning function (`clean_output`) to strip anomalous AI tokens, a garbage detector (`is_garbage`), and a fuzzy-matching `GOLDEN_DICT` to intercept failures and serve perfect translations for highly complex or well-known inputs.

### 3. The Deep Learning Model (Style Transfer)
* **Technology:** Hugging Face Transformers, `google/mT5-small`, PEFT (LoRA).
* **Location:** The model weights are loaded locally from the `./sanskritnet_final` directory. If the local weights are missing (e.g., due to a fresh Colab session), the system automatically fetches the base `mT5-small` model from Hugging Face as a fallback.
* **How it works:** The model is an encoder-decoder transformer. It was fine-tuned using Low-Rank Adaptation (LoRA) on over 500 cross-validated *Ramcharitmanas* verse triplets. It takes the Stage 1 Hindi output as an input sequence and auto-regressively generates Awadhi text that matches the poetic rhythm of Tulsidas.

---

## Installation and Running the Code

This application is optimized to run locally or in cloud environments like Google Colab. It features a robust **CPU-fallback mode**, meaning the application will successfully compile and run even if GPU limits are reached.

**Step-by-Step Execution Guide** 

Option A: Running in Google Colab (Recommended) Because the application handles GPU availability automatically (falling back to CPU if limits are reached), Colab is the easiest way to run the project.Open a new Google Colab Notebook.Upload the sanskritnet_final folder to the Colab files section.Paste the contents of app.py into a notebook cell.Run the cell. The dependencies will install, and a public gradio.live link will appear at the bottom.

Option B: Running LocallyClone the repository to your local machine:Bashgit clone [https://github.com/YOUR_USERNAME/SanskritNet.git](https://github.com/YOUR_USERNAME/SanskritNet.git)
cd SanskritNet

Set up a virtual environment (optional but recommended):Bash   python -m venv venv
   source venv/bin/activate  # On Windows use: venv\Scripts\activate
   
Install the dependencies:Bashpip install -r requirements.txt  # Or use the pip install command listed above

Run the application:Bashpython app.py

Access the Web App: Open your web browser and go to the local host address provided in your terminal (usually http://127.0.0.1:7860). 

How to Use the InterfaceOnce the application is running, you will be greeted with three main tabs: 

Sanskrit → Awadhi:Paste a Sanskrit verse into the input box.Click the "Translate" button.The app will process the text through the Stage 1 API (Sanskrit $\rightarrow$ Hindi) and Stage 2 Model (Hindi $\rightarrow$ Awadhi). 

Hindi → Awadhi:If you want to bypass the API, type a modern Hindi sentence directly into the input box to see how the model transfers the poetic style. 

Batch Evaluation:Click "Run All Demo Shlokas" to automatically process a pre-loaded list of verses. This pulls data and generates a clean table displaying the original text, intermediate translations, and final Awadhi outputs.
