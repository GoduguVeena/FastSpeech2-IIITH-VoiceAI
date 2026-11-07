# FastSpeech2 for English and Telugu Text-to-Speech Synthesis

**IIITH Speech Processing Lab – Assignment**

An implementation and language-adaptation project exploring **FastSpeech 2**, a non-autoregressive text-to-speech (TTS) architecture, for English and Telugu speech synthesis.

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-FastSpeech2-ee4c2c.svg)](https://pytorch.org/)
[![Languages](https://img.shields.io/badge/Languages-English%20%7C%20Telugu-green.svg)](#project-overview)
[![Status](https://img.shields.io/badge/Project-In%20Progress-orange.svg)](#implementation-status)

---

## Project Overview

The objective of this project is to implement and adapt FastSpeech 2 independently for two languages:

- **English:** Svarah dataset
- **Telugu:** IndicTTS Telugu dataset

FastSpeech 2 generates speech directly from text using a duration-based variance adaptor. It incorporates duration, pitch, and energy information to improve speech expressiveness and synthesis control.

This project investigates the language-specific requirements involved in adapting the architecture, particularly the differences between English phoneme-based text representation and Telugu Unicode character-level tokenization.

The implementation is based on the original FastSpeech 2 research and the reference implementation by [ming024/FastSpeech2](https://github.com/ming024/FastSpeech2).

## Objectives

- Study and implement the FastSpeech 2 architecture.
- Prepare separate English and Telugu datasets.
- Adapt text preprocessing and tokenization for both languages.
- Investigate duration, pitch, and energy representations.
- Train separate language-specific FastSpeech 2 models.
- Generate speech for the prescribed test sentences.
- Analyze training behavior, generated speech, and variance adaptor outputs.
- Document the implementation, adaptations, experiments, and limitations.

## Architecture

FastSpeech 2 follows a non-autoregressive architecture consisting of the following major components:

1. **Text Encoder:** Converts input text or phoneme sequences into hidden representations.
2. **Variance Adaptor:** Incorporates duration, pitch, and energy information.
3. **Length Regulator:** Expands encoder representations according to predicted or ground-truth phoneme durations.
4. **Mel-Spectrogram Decoder:** Generates the output mel-spectrogram.
5. **Vocoder:** Converts the mel-spectrogram into a waveform.

### Architecture flow

```text
                  Input Text
                      |
                      v
               Text Processing
                      |
                      v
                 Text Encoder
                      |
                      v
               Variance Adaptor
              /       |        \
             /        |         \
        Duration     Pitch      Energy
             \        |         /
              \       |        /
                      v
                Length Regulator
                      |
                      v
                Mel Decoder
                      |
                      v
               Mel-Spectrogram
                      |
                      v
                   Vocoder
                      |
                      v
                Generated Audio
```

## Datasets

Two independent datasets are used for the language-specific experiments.

| Component | English | Telugu |
|---|---|---|
| Dataset | Svarah | IndicTTS Telugu |
| Source | AI4Bharat | SPRINGLab |
| Text representation | ARPAbet phonemes | Unicode characters |
| Audio preparation | 22,050 Hz | 22,050 Hz |
| Training duration target | Approximately 5 hours | Approximately 5 hours |
| Validation duration target | Approximately 1 hour | Approximately 1 hour |

### 1. English – Svarah

Dataset: [AI4Bharat Svarah](https://huggingface.co/datasets/ai4bharat/Svarah)

Dataset preparation completed:

- Scanned the available recordings and metadata.
- Selected a deterministic training and validation subset using seed 42.
- Prepared a training subset of 3,453 recordings (approximately 5 hours).
- Prepared a validation subset of 695 recordings (approximately 1 hour).
- Kept unused recordings separate.
- Verified that training and validation record IDs do not overlap.
- Extracted audio recordings and checked extraction errors.
- Resampled audio to 22,050 Hz.
- Applied English grapheme-to-phoneme conversion using `g2p-en`.
- Generated phonemized metadata for the selected recordings.

**English text representation:** ARPAbet phoneme sequences.

A 20-record forced-alignment experiment was also conducted using Montreal Forced Aligner (MFA) with an English acoustic model. This is an initial alignment experiment, not full-corpus alignment.

### 2. Telugu – IndicTTS Telugu

Dataset: [SPRINGLab IndicTTS Telugu](https://huggingface.co/datasets/SPRINGLab/IndicTTS_Telugu)

Dataset preparation completed:

- Scanned the available recordings and metadata.
- Selected a deterministic subset using seed 42.
- Prepared 1,168 training recordings (approximately 5 hours).
- Prepared 199 validation recordings (approximately 1 hour).
- Kept unused recordings separate.
- Verified that training and validation record IDs do not overlap.
- Extracted and validated the selected recordings.
- Resampled audio to 22,050 Hz.
- Validated transcript availability and Unicode normalization.

**Telugu text representation:** Unicode character-level tokenization.

A Telugu character vocabulary was constructed with 73 tokens, including padding and unknown-token entries. Encoding and decoding tests were performed to verify exact transcript reconstruction on the tested samples.

### Dataset preparation summary

| Validation check | English | Telugu |
|---|---:|---:|
| Training recordings | 3,453 | 1,168 |
| Validation recordings | 695 | 199 |
| Training duration | ~5 hours | ~5 hours |
| Validation duration | ~1 hour | ~1 hour |
| Audio resampling | Completed | Completed |
| Text preprocessing | Completed | Completed |
| Train/validation overlap check | Passed | Passed |

The selected subsets are deterministic and are documented through the corresponding metadata and manifests. Raw dataset recordings are not included in this repository.

## Language-Specific Adaptations

### English

- Grapheme-to-phoneme conversion using `g2p-en`.
- ARPAbet phoneme representation.
- Phoneme sequence validation against the model's symbol inventory.
- Initial forced-alignment testing using MFA.
- Custom pronunciation entries investigated for words not covered by the initial dictionary.

### Telugu

- Unicode NFC normalization.
- Character-level tokenization.
- Custom Telugu character vocabulary.
- Modified vocabulary loading in the dataset implementation.
- Updated model vocabulary-size handling.
- XLS-R-based CTC character-alignment experiments.
- Investigation of character-duration label construction and alignment reliability.

**Important alignment limitation:** CTC character timestamps are approximate emission regions, not verified phone-level or character-level duration ground truth. A midpoint-based duration partition was tested experimentally, but the resulting duration labels have not been finalized for model training. Records with questionable alignment quality require further review.

## Repository Structure

```text
FastSpeech2-IIITH-VoiceAI/
│
├── English/
│   ├── config/
│   │   ├── model.yaml
│   │   ├── preprocess.yaml
│   │   └── train.yaml
│   ├── data/
│   ├── audio/
│   ├── checkpoints/
│   ├── hifigan/
│   ├── model/
│   ├── preprocessor/
│   ├── text/
│   ├── transformer/
│   ├── utils/
│   ├── dataset.py
│   ├── prepare_align.py
│   ├── preprocess.py
│   ├── train.py
│   ├── synthesize.py
│   ├── inference.py
│   ├── test_sentences.txt
│   ├── requirements.txt
│   └── README.md
│
├── Telugu/
│   ├── config/
│   │   ├── model.yaml
│   │   └── preprocess.yaml
│   ├── data/
│   ├── audio/
│   ├── checkpoints/
│   ├── hifigan/
│   ├── model/
│   ├── preprocessor/
│   ├── text/
│   ├── transformer/
│   ├── utils/
│   ├── dataset.py
│   ├── prepare_align.py
│   ├── preprocess.py
│   ├── train.py
│   ├── synthesize.py
│   ├── inference.py
│   ├── test_sentences.txt
│   ├── README.md
│   └── ...
│
├── Results/
│   ├── English/
│   │   ├── training_loss.png
│   │   ├── validation_loss.png
│   │   ├── test_generated_audios/
│   │   └── variance_adaptor_plots/
│   │       ├── duration/
│   │       ├── pitch/
│   │       └── energy/
│   │
│   └── Telugu/
│       ├── training_loss.png
│       ├── validation_loss.png
│       ├── test_generated_audios/
│       └── variance_adaptor_plots/
│           ├── duration/
│           ├── pitch/
│           └── energy/
│
├── Report/
│   └── Technical_Report.pdf
│
├── requirements.txt
├── LICENSE-FASTSPEECH2-UPSTREAM
├── THIRD_PARTY_NOTICES.md
└── README.md
```

*The structure describes the intended project organization. Some result directories are currently placeholders; the corresponding training and inference artifacts have not yet been generated.*

## Assignment Test Sentences

The prescribed test sentences are maintained separately:

- [`English/test_sentences.txt`](English/test_sentences.txt)
- [`Telugu/test_sentences.txt`](Telugu/test_sentences.txt)

These files preserve the assignment-provided wording, including unusual spacing where present in the source. They are intended for subsequent inference and evaluation.

## Implementation Status

The repository is being developed incrementally. The following table distinguishes completed preparation work from pending model implementation.

| Component | English | Telugu |
|---|---|---|
| Dataset inspection | Completed | Completed |
| Dataset selection and split | Completed | Completed |
| Audio extraction and resampling | Completed | Completed |
| Text preprocessing | Completed | Completed |
| Language-specific tokenization | Completed | Completed |
| Initial alignment experiments | Partial | Experimental |
| Final duration labels | Pending | Pending |
| Complete dataset integration | Pending | Pending |
| FastSpeech 2 training | Pending | Pending |
| Validation and loss analysis | Pending | Pending |
| Trained checkpoints | Pending | Pending |
| Inference implementation and testing | Pending | Pending |
| Five generated test audios | Pending | Pending |
| Duration, pitch, and energy plots | Pending | Pending |
| Final comparative evaluation | Pending | Pending |

**Current status:** Dataset preparation and initial language-specific experiments have been completed. Model training and inference are not yet complete. Empty checkpoint and results directories are placeholders, not trained model outputs.

## Technical Report

The implementation status, dataset preparation methodology, language-specific adaptations, alignment experiments, limitations, and pending work are documented in:

[`Report/Technical_Report.pdf`](Report/Technical_Report.pdf)

The report will be updated as the implementation and evaluation progress.

## Installation

The repository contains a preliminary dependency list. A fully validated, pinned environment will be documented after end-to-end preprocessing and training are operational.

Clone the repository:

```bash
git clone https://github.com/GoduguVeena/FastSpeech2-IIITH-VoiceAI.git
cd FastSpeech2-IIITH-VoiceAI
```

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it:

**Windows**
```bash
.venv\Scripts\activate
```

**Linux/macOS**
```bash
source .venv/bin/activate
```

Install the preliminary dependencies:

```bash
pip install -r requirements.txt
```

> Installation alone does not make both language pipelines ready to run. Dataset-specific preprocessing, configuration integration, duration-label validation, and end-to-end testing remain pending.

## Reproducibility

The project uses deterministic dataset selection with seed 42. Language-specific metadata, vocabulary information, configuration files, and preparation notes are maintained in the repository.

For reproducibility:

- Use the corresponding language-specific configuration.
- Obtain the datasets from their original sources.
- Preserve the documented training and validation splits.
- Keep raw datasets and large model checkpoints outside Git where appropriate.
- Record model parameters, environment versions, and training settings when experiments are run.
- Do not interpret experimental alignment timestamps as verified ground-truth durations.

## References

1. Ren, Y., et al. **FastSpeech 2: Fast and High-Quality End-to-End Text to Speech.** International Conference on Learning Representations (ICLR), 2021. [Paper](https://arxiv.org/abs/2006.04558)
2. [Original FastSpeech 2 implementation – ming024/FastSpeech2](https://github.com/ming024/FastSpeech2)
3. [AI4Bharat Svarah dataset](https://huggingface.co/datasets/ai4bharat/Svarah)
4. [SPRINGLab IndicTTS Telugu dataset](https://huggingface.co/datasets/SPRINGLab/IndicTTS_Telugu)
5. [Montreal Forced Aligner](https://montreal-forced-aligner.readthedocs.io/)
6. [Hugging Face Transformers](https://huggingface.co/docs/transformers/index)

## Acknowledgements

This project uses the original FastSpeech 2 implementation as its architectural reference. The upstream implementation is credited to its original authors and is distributed under its original license, retained in this repository.

Dataset providers and open-source tool developers are acknowledged for making speech research and experimentation accessible.

## Project Maintainer

**Godugu Veena**

B.Tech – Computer Science and Engineering (Data Science)

Rajeev Gandhi Memorial College of Engineering and Technology (RGMCET)

GitHub: [@GoduguVeena](https://github.com/GoduguVeena)

---

*This repository documents an ongoing implementation. Claims about training, checkpoints, generated audio, and evaluation will be added only after those experiments have actually been completed.*
