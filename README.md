# Audio Analyst – 911 Emergency Call Processing

## Overview

This module processes 911 emergency call audio and extracts structured information including incident type, location, sentiment (calm vs distressed), and urgency score.

The pipeline uses:

* Whisper for speech-to-text
* spaCy for named entity recognition
* Hugging Face model for sentiment analysis

---

## Project Structure

```text
audio-incident-analyzer/
├── audio_analyzer.py
├── requirements.txt
├── README.md
├── data/
│   └── (place dataset here)
└── output/
```

---

## Dataset

This project uses the dataset:

**911 Recordings: The First 6 Seconds**

### Download

1. Go to:
   https://www.kaggle.com/datasets/louisteitelbaum/911-recordings-first-6-seconds

2. Download the dataset (requires Kaggle account)

3. Extract the files

---

### Setup Data Folder

Place all `.wav` files and `911_metadata.csv` inside:

```text
data/
```

Example:

```text
data/
├── 911_metadata.csv
├── call_1.wav
├── call_2.wav
├── ...
```

---

## Setup

Install dependencies:

```bash
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

Ensure `ffmpeg` is installed (required for Whisper).

---

## Usage

### Run on 10 files (quick test)

```bash
python audio_analyzer.py --max 10 --summary
```

### Run on full dataset

```bash
python audio_analyzer.py
```

### Use smaller model (faster)

```bash
python audio_analyzer.py --max 10 --whisper_model tiny
```

---

## Output

The script generates:

```text
output/audio_output.csv
```

Columns:

* Call_ID
* Transcript
* Extracted_Event
* Location
* Sentiment
* Urgency_Score

---

## Sample Output

| Call_ID | Transcript                                        | Extracted_Event      | Location    | Sentiment  | Urgency_Score |
| ------- | ------------------------------------------------- | -------------------- | ----------- | ---------- | ------------- |
| C001    | Man.                                              | Unknown              | Indiana     | Calm       | 0.14          |
| C002    | This is the Blocopia Bank in 1302 with the Tra... | Robbery / theft      | Florida     | Distressed | 0.49          |
| C003    | Yeah? Yeah. Yeah. Yeah. Yeah. Yeah.               | Domestic disturbance | Wisconsin   | Calm       | 0.00          |
| C004    | Yes, we're at the Nelson building and there's ... | Shooting             | Florida     | Calm       | 0.30          |
| C005    | Listen, we're playing really bad at this. Okay... | Domestic disturbance | Connecticut | Distressed | 0.70          |


---

## Pipeline

```text
Audio → Transcription → Information Extraction → Sentiment & Urgency → CSV Output
```

---

## Notes

* Audio clips are short (~6 seconds), so transcripts may be incomplete
* Very short transcripts are labeled as "Unknown" event
* Results depend on audio quality and speech clarity
