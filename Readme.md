# Project Overview and Code Workflow

## Project Overview
This project aims to conduct a comprehensive comparison of different Question Answering (QA) model paradigms—extractive, generative, and retrieval-augmented—on the Stanford Question Answering Dataset (SQuAD). The experiments leverage two major language model APIs (OpenAI and Google’s Gemini) to evaluate both "stuff" and "map-reduce" prompting pipelines. Key objectives include:

- **Accuracy**: Measure exact match (EM) and F1 score across model–pipeline combinations.
- **Efficiency**: Track API latency and token usage to quantify cost vs. performance trade-offs.
- **Scalability**: Demonstrate how chunking strategies (map-reduce) affect results on long contexts.

## Working of the Code
The repository is organized into four Jupyter notebooks—two for each API (OpenAI and Gemini)—and follows a consistent structure:

1. **Data Loading & Preprocessing**
   - Load the SQuAD dataset.
   - Preprocess contexts and questions.
   - For the map-reduce pipeline: split long contexts into fixed-size chunks with overlap.

2. **Prompt Construction**
   - **Stuff Pipeline**: Concatenate the full context with the question into a single prompt template.
   - **Map-Reduce Pipeline**:
     - **Map Step**: Send each chunk individually with the question prompt to the model.
     - **Reduce Step**: Aggregate chunk-level responses by ranking or simple concatenation, then finalize an answer prompt to refine the aggregated response.

3. **API Calls & Integration**
   - Notebooks:
     - `openai_stuff.ipynb` / `openai_map_reduce.ipynb`
     - `gemini_stuff.ipynb` / `gemini_map_reduce.ipynb`
   - Common functionality encapsulated in utility functions:
     - `generate_answer_stuff(model, context, question)`
     - `generate_answer_map_reduce(model, chunks, question)`
   - Models are parameterized (e.g., model name, temperature, max tokens). Responses and metadata (latency, tokens) are logged.

4. **Evaluation & Metrics**
   - Calculate Exact Match (EM) and F1 score by comparing model outputs to ground-truth answers.
   - Record API call latency and token consumption (prompt + completion tokens).
   - Results stored in structured CSV files for downstream analysis.

5. **Analysis & Visualization**
   - Load results CSVs and generate comparative plots:
     - Accuracy vs. Pipeline for each model API.
     - Latency vs. Pipeline.
     - Token usage vs. Accuracy trade-off.
   - Visualizations created using Matplotlib for clear interpretation.

---
*End of README overview.*  
