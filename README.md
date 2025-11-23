import re

# ------------------------------------------
# SKILL DATABASE (You can add more)
# ------------------------------------------
SKILLS = [
    "python", "java", "c++", "sql", "excel", "machine learning",
    "data analysis", "power bi", "salesforce", "javascript",
    "html", "css", "communication", "leadership", "project management"
]

def extract_text(file_path):
    """Reads text from a .txt resume file."""
    # NOTE: This function requires the file 'sample_resume.txt' to exist
    with open(file_path, "r", encoding="utf-8") as f:
        return f.read().lower()

def find_skills(text):
    """Checks skills inside resume text."""
    found = [skill for skill in SKILLS if skill in text]
    missing = [skill for skill in SKILLS if skill not in text]
    return found, missing

def score_resume(found_skills):
    """5 points per skill found (max 100)."""
    return min(100, len(found_skills) * 5)

def generate_summary(text):
    """Creates a simple summary from first sentence."""
    # Use re.split to handle common sentence delimiters
    sentences = re.split(r"[.!?]", text)
    if sentences:
        # Get the first sentence, strip whitespace, and capitalize the start
        first_sentence = sentences[0].strip()
        if first_sentence:
            return f"Profile Summary: {first_sentence.capitalize()}..."
    return "Profile Summary: Not available."

def analyze_resume(file_path):
    text = extract_text(file_path)
    found, missing = find_skills(text)
    score = score_resume(found)
    summary = generate_summary(text)

    return {
        "found_skills": found,
        "missing_skills": missing,
        "score": score,
        "summary": summary
    }

if __name__ == "__main__":
    print("Analyzing resume...\n")
    
    # Ensure 'sample_resume.txt' is in the same directory as this script!
    try:
        result = analyze_resume("sample_resume.txt")
        
        print("===== RESUME ANALYSIS RESULT =====")
        print("Skills Found:", result["found_skills"])
        print("Missing Skills:", result["missing_skills"])
        print("Score:", result["score"])
        print(result["summary"])
    except FileNotFoundError:
        print("ERROR: 'sample_resume.txt' not found. Please create the file with content.")

    
