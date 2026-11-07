# FastSpeech 2 for English and Telugu

An ongoing implementation and adaptation of FastSpeech 2 for:
- English: AI4Bharat Svarah
- Telugu: SPRINGLab IndicTTS Telugu

## Project status

This repository documents the implementation and dataset preparation
experiments. It is not yet a completed or trained TTS system.

### Completed
- Dataset selection and approximately 5-hour training / 1-hour
  validation splits for both languages.
- Audio extraction and resampling to 22,050 Hz.
- English transcript phonemization using G2P.
- Telugu Unicode character vocabulary and tokenizer.
- English forced-alignment experiments on a 20-record subset.
- Telugu CTC character-alignment experiments and validation.

### Pending
- Reliable dataset-specific duration alignment and preprocessing.
- Generation and validation of mel, pitch, energy and duration features.
- FastSpeech 2 training for both languages.
- Checkpoint generation and inference.
- Required test audio samples, variance-adaptor plots and final evaluation.

No training results, generated samples or loss curves are claimed in
this repository unless explicitly added with supporting artifacts.

## Repository structure

- `English/`: English implementation, configuration and data manifests.
- `Telugu/`: Telugu tokenizer adaptation, implementation and alignment
  experiment artifacts.
- `Results/`: Reserved for actual generated results.
- `Report/`: Technical report.

## Dataset handling

The original audio datasets are not included in this repository.
Refer to the respective dataset providers for access and licensing.

Dataset splits and experiment manifests are retained selectively.
Local absolute paths should be replaced with paths appropriate to
the user's environment.

## Implementation basis

The project builds on the FastSpeech 2 implementation by
[ming024/FastSpeech2](https://github.com/ming024/FastSpeech2)
and the paper [FastSpeech 2](https://arxiv.org/abs/2006.04558).

## Reproducibility

The code and configuration are under active development.
The current repository snapshot does not yet provide a complete
end-to-end preprocessing, training or inference command for either
language. See the language-specific README files for details.
