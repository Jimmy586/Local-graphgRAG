## 📦 Installation and Setup

Follow these steps to set up and use GraphRag with local models provided by Ollama :

_Warning :  You are expected to run a new project following these instructions_

1. **Create and activate a new conda environment:**
    ```bash
    conda create -n graphRAG python=3.12
    conda activate graphRAG
    ```

2. **Install Ollama:**
    - Visit [Ollama's website](https://ollama.com/) for installation instructions.
    - Or, run:
    ```bash
    pip install ollama
    ```

3. **Download the required models using Ollama, we can choose any llm and embedding model provided under Ollama:**
    ```bash
    ollama pull gemma2  #llm
    ```
   API endpoint : http://localhost:11434/v1
   - (Option 1) You could use an embedding model from Ollama by running :
   ```bash
    ollama pull nomic-embed-text  #embedding
    ```
    - (Option 2) [**RECOMMENDED**] Download LM Studio at : https://lmstudio.ai/
      - Download from LM Studio the model : nomic-ai/nomic-embed-text-v1.5-GGUF/nomic-embed-text-v1.5.Q4_K_M.gguf
      - Go to the "Local Server" section, then "Embedding Model Settings"
      - Select your Embedding Model
      - Start Server
      - The API endpoint should be : http://localhost:1234/v1

4. **Create the required input directory: This is where the experiments data and results will be stored - ./ragtest**
    ```bash
    mkdir -p ./ragtest/input
    ```
    
5. **Install all required packages :**
    ```bash
   pip install graphrag
   pip install -r requirements.txt
    ```
   _Warning :  This project requires a C++ version 14 or more_
6. **Add your own data in the ./ragtest/input in .txt format.**

7. **Initialize the ./ragtest folder to create the required files:**
    ```bash
    python -m graphrag.index --init --root ./ragtest
    ```
_Warning :  This will create the settings.yaml file in which you should update the Local Model specs for to model to use them.(copy the one in this project)_
8. **Move the settings.yaml file, this is the main predefined config file configured with ollama local models :**
    ```bash
    mv settings.yaml ./ragtest
    ```

Users can experiment by changing the models. The llm model expects language models like llama3, mistral, phi3, etc., and the embedding model section expects embedding models like mxbai-embed-large, nomic-embed-text, etc., which are provided by Ollama or LM Studio. You can find the complete list of models provided by Ollama here https://ollama.com/library, which can be deployed locally. The default API base URLs are http://localhost:11434/v1 for LLM and http://localhost:11434/api for embeddings if using Ollama and  http://localhost:1234/v1 if using LM Studio, hence they are added to the respective sections in settings.yaml. 

9. **Run the indexing, which creates a graph:**
    ```bash
    python -m graphrag.index --root ./ragtest
    ```

12. **Run a query: Only supports Global method** 
    ```bash
    python -m graphrag.query --root ./ragtest --method global "What is machinelearning?"
    ```

**Graphs can be saved which further can be used for visualization by changing the graphml to "true" in the settings.yaml :**
    
    snapshots:
    graphml: true
    
**To visualize the generated graphml files, you can use : https://gephi.org/users/download/ or the script provided in the repo visualize-graphml.py :**

Pass the path to the .graphml file to the below line in visualize-graphml.py:

    graph = nx.read_graphml('./ragtest/output/20240716-115846/artifacts/summarized_graph.graphml') 

11**Visualize .graphml :**

    ```bash
    python visualize-graphml.py
    ```