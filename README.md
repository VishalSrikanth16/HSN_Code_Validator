from google.adk.agents import Agent
import pandas as pd
from pathlib import Path
from functools import lru_cache
from dotenv import load_dotenv
import re

load_dotenv()

BASE_DIR = Path(__file__).parent
excel_path = BASE_DIR / 'HSN_SAC.xlsx'

# Reading the excel file that is locally stored using pandas library
df = pd.read_excel(excel_path, dtype={'\nHSNCode': str})
df.columns = df.columns.str.strip()

df['Description'] = df['Description'].fillna("").astype(str)

# Build HSN code -> description dictionary (zero-padded code strings)
HSN_DATA = dict(zip(df['HSNCode'].str.zfill(2), df['Description']))

@lru_cache(maxsize=1024)
def get_hsn_info(query: str) -> dict:
    """
    Given a query (HSN code or description substring)
    returns matching HSN codes and descriptions
    
    """
    query_clean = query.strip().lower()

    # Check if query looks like an HSN code for ex. numerical values/integer values.
    if re.fullmatch(r'\d{2,}', query_clean):
        code = query_clean.zfill(2)
        description = HSN_DATA.get(code)
        if description:
            return {
                "status": "success",
                "report": f"HSN Code {code}: {description}."
            }
        else:
            return {
                "status": "error",
                "error_message": f"No data found for HSN code '{code}'."
            }
    else:

        matches = []
        for code, desc in HSN_DATA.items():
            if query_clean in desc.lower():
                matches.append(f"{code}: {desc}")

        if matches:
            report = "Matching HSN codes:\n" + "\n".join(matches) # match the codes
            return {
                "status": "success",
                "report": report
            }
        else:
            return {
                "status": "error",
                "error_message": f"No HSN codes found matching description '{query}'." # if code does not exist
            }

root_agent = Agent(
    name="hsn_lookup_agent",
    model="gemini-2.0-flash",  
    description="Agent to provide information about HSN codes and tax rates.",
    instruction=(
        "You are a helpful assistant who provides GST details for HSN codes. "
        "Users may ask about HSN codes or descriptions. "
        "If user provides an HSN code, return its description. "
        "If user provides a description, return matching HSN codes."
    ),
    tools=[get_hsn_info],
)
