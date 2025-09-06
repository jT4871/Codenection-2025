# Codenection-2025
import re
import difflib
from datetime import datetime
from typing import Tuple, List

Vehicle_Database = [
    ("Toyota","Vios",2010,2025),
    ("Toyota","Camry",2005,2025),
    ("Honda","City",2008,2025),
    ("Honda","Civic",2000,2025),
    ("Perodua","Myvi",2005,2025),
    ("Perodua","Axia",2014,2025),
    ("Proton","Saga",2000,2025),
    ("Proton","X70",2019,2025),
]

# Get data from Database
VALID_BRANDS = sorted(set([brand for brand, _, _, _ in VEHICLE_DB]))
VALID_MODELS = sorted(set([model for _, model, _, _ in VEHICLE_DB]))

def only_alphabet(s: str) -> bool:
    return bool(re.match(r'^[a-zA-Z\s]+$', s))

def suggest_correction(user_input: str, valid_list: List[str]) -> str:
    matches = difflib.get_close_matches(user_input, valid_list, n=1, cutoff=0.6)
    return matches[0] if matches else user_input

def validate_vehicle_data(brand: str, model: str, produce_year: int, renew_year: int) -> Tuple[bool, str]:
    errors = []
    current_year = datetime.now().year
   
  #correction and check brands
    corrected_brand = suggest_correction(brand, VALID_BRANDS)
    if corrected_brand != brand and corrected_brand in VALID_BRANDS:
        return False, f"Brand '{brand}' fail，do you mean '{corrected_brand}'？"
    elif corrected_brand not in VALID_BRANDS:
        errors.append(f"Drand '{brand}' undefind")
        
  #correction and check model
    corrected_model = suggest_correction(model, VALID_MODELS)
    if corrected_model != model and corrected_model in VALID_MODELS:
        return False, f"Model '{model}' fail，do you mean '{corrected_model}'？"
    elif corrected_model not in VALID_MODELS:
        errors.append(f"Model '{model}' undefind")

   #year produce     
   if not isinstance(produce_year, int):
        errors.append(f"year '{produce_year}' must be interger")
    elif produce_year < 1886:
        errors.append(f"produce year '{produce_year}' can't early than 1866")
    elif produce_year > current_year + 1:
        errors.append(f"produce year '{produce_year}' can't late than{current_year + 1}")

  #renew lisence year
    if not isinstance(renew_year, int):
        errors.append(f"Renew year '{renew_year}' must be interger")
    elif renew_year < produce_year:
        errors.append(f"Renew year '{renew_year}' can't early than '{produce_year}'")
    elif renew_year > current_year + 1:
        errors.append(f"Renew year '{renew_year}' can't late than{current_year}")
    
  #check data
    if (brand, model, produce_year, renew_year) not in VEHICLE_DB:
        errors.append(f"Data ({brand}, {model}, {produce_year}, {renew_year}) undefind")
    if errors:
        return False, "；".join(errors)
    return True, "Success"

