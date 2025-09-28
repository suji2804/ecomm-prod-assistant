# Ecomm Product Assistant

## Overview
Ecomm Product Assistant is an intelligent chatbot platform for product review and price assistance. It leverages data scraping, vector search, agentic workflows, and cloud-native deployment to deliver fast, relevant answers about e-commerce products.

## Features
- **Streamlit UI for Data Scraping:** Scrape product reviews and prices from Flipkart.
- **Vector Database (Astra DB):** Store and index product data for fast retrieval.
- **MMR Retrieval:** Use Maximal Marginal Relevance for relevant document selection.
- **RAGAS Evaluation:** Evaluate retrieval quality and relevance.
- **Agentic AI Workflow (LangGraph):** Orchestrate retrieval, generation, and web search tools for robust Q&A.
- **Web Search Fallback:** If product data is missing, fetch answers from the web.
- **FastAPI Chat UI:** Chatbot interface for users to ask about product reviews and prices.
- **Cloud-Native Deployment:** Automated CI/CD with GitHub Actions, deployed to AWS EKS using CloudFormation.

## Architecture
- **Data Ingestion:**
  - Use Streamlit UI (`scrapper_ui.py`) to scrape Flipkart data.
  - Save scraped data to Astra DB (`data_ingestion.py`).
- **Retrieval & Evaluation:**
  - Retrieve relevant documents using MMR (`retrieval.py`).
  - Evaluate results with RAGAS (`ragas_eval.py`).
- **Agentic Workflow:**
  - LangGraph-based workflow (`agentic_rag_workflow.py`, `agentic_workflow_with_mcp_websearch.py`) routes queries to retrieval, generation, or web search tools.
- **Chatbot API:**
  - FastAPI backend (`router/main.py`) serves the chat assistant.
  - Frontend templates in `templates/`.
- **Deployment:**
  - GitHub Actions workflow (`.github/workflows/deploy.yml`) builds and pushes Docker images to ECR, deploys to AWS EKS using manifests in `k8/`.

## Getting Started
1. **Clone the repository:**
   ```
   git clone <repo-url>
   cd ecomm-prod-assistant
   ```
2. **Set up Python environment:**
   ```
   python -m venv venv
   .\venv\Scripts\activate
   pip install -r requirements.txt
   ```
3. **Configure environment variables:**
   - Copy `.env.example` to `.env` and fill in your API keys and Astra DB details.
4. **Run Streamlit UI for data scraping:**
   ```
   streamlit run scrapper_ui.py
   ```
5. **Start FastAPI server:**
   ```
   uvicorn prod_assistant.router.main:app --reload
   ```
6. **Access the chatbot UI:**
   - Open your browser at `http://localhost:8000`

## Deployment
- **CI/CD:**
  - On every push to `main`, GitHub Actions builds and pushes the Docker image to ECR, then deploys to AWS EKS.
- **Kubernetes:**
  - Manifests in `k8/` define deployment and service.
  - Secrets are managed via `kubectl` and GitHub Actions.

## Project Structure
```
├── prod_assistant/
│   ├── etl/                # Data ingestion and scrapping
│   ├── retriever/          # Retrieval logic
│   ├── workflow/           # Agentic workflows
│   ├── router/             # FastAPI endpoints
│   ├── evaluation/         # RAGAS evaluation
│   ├── utils/              # Config/model loading
│   ├── logger/             # Logging
│   ├── exception/          # Custom exceptions
├── scrapper_ui.py          # Streamlit UI for scraping
├── requirements.txt
├── Dockerfile
├── k8/                     # Kubernetes manifests
├── templates/              # Frontend templates
├── .github/workflows/      # CI/CD workflows
```

## License
MIT

## Authors
- suji2804

## Acknowledgements
- LangChain, LangGraph, Streamlit, Astra DB, FastAPI, RAGAS, AWS EKS
