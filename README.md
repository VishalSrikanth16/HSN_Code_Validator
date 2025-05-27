Overview:

This project is an AI-powered agent built using Google's Agent Development Kit (ADK) that provides quick information about HSN (Harmonized System of Nomenclature) codes. Users can query by either an HSN code or a product/service description, and the agent returns matching HSN codes and their descriptions. It supports GST-related lookups for tax classification purposes. The agent reads HSN data from an Excel file (HSN_SAC.xlsx), processes it into a searchable format, and responds intelligently based on user queries.

Features:

Lookup HSN code descriptions by exact HSN code.

Search HSN codes by partial or full product/service descriptions.

Handles missing or incomplete data gracefully.

Built on Google ADK with the Gemini language model.

Fast and efficient in-memory data lookup.

Designed as a reusable agent for integration or deployment.

Project Structure:

ROOT FOLDER/
│
├── HSN_Agent/
│   ├── __init__.py
│   ├── agent.py            # Main agent code with HSN lookup logic
│   ├── HSN_SAC.xlsx        # Excel file containing HSN code data
│   └── .env                # Environment variables (API keys etc)
│
├── README.md               # This file
└── requirements.txt        # Python dependencies

Setup Instructions: 

1. Clone the repository

git clone https://github.com/yourusername/HSN_Code_Validator.git
cd HSN_Code_Validator/HSN_Agent

2. Create and activate a virtual environment

python -m venv venv
source venv/bin/activate      # macOS/Linux
venv\Scripts\activate         # Windows

3. Install dependencies

pip install -r ../requirements.txt

4. Configure environment variables

GOOGLE_GENAI_USE_VERTEXAI=FALSE
GOOGLE_API_KEY=your_google_api_key_here

5. Run the agent

adk web

Usage:

Query the agent by providing an HSN code (e.g., 1001) to get its description.

Or query by product/service description (e.g., "wheat") to get all matching HSN codes.

The agent will return results or a helpful error message if no match is found.

License:

This project is licensed under the MIT License.