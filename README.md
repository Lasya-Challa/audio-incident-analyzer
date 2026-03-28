# Audio Analyst – 911 Emergency Call Processing

## Overview

This module processes 911 emergency call audio and extracts structured information including incident type, location, sentiment (calm vs distressed), and urgency score.

The pipeline uses:

* Whisper for speech-to-text
* spaCy for named entity recognition
* Hugging Face model for sentiment analysis

---

## Project Structure

```
audio/
├── audio_analyzer.py
├── requirements.txt
├── README.md
├── data/
│   ├── 911_metadata.csv   (optional)
│   ├── *.wav files
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

Place the files inside:

```
audio/data/
```

Final structure should look like:

```
audio/data/
├── 911_metadata.csv
├── call_1.wav
├── call_2.wav
├── ...
```

---

## Setup

Install dependencies:

```
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

Ensure `ffmpeg` is installed (required for Whisper).

---

## Usage

### Run on 10 files (quick test)

```
python audio_analyzer.py --max 10 --summary
```

### Run on full dataset

```
python audio_analyzer.py
```

### Use smaller model (faster)

```
python audio_analyzer.py --max 10 --whisper_model tiny
```

---

## Output

The script generates:

```
audio/output/audio_output.csv
```

Columns:

* Call_ID
* Transcript
* Extracted_Event
* Location
* Sentiment
* Urgency_Score

---

## Pipeline

```
Audio → Transcription → Information Extraction → Sentiment & Urgency → CSV Output
```

---

## Notes

* Audio clips are short (~6 seconds), so transcripts may be incomplete
* Very short transcripts are labeled as "Unknown" event
* Results depend on audio quality and speech clarity
