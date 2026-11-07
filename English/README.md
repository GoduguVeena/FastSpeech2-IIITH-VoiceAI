# English FastSpeech 2 Adaptation

## Dataset
AI4Bharat Svarah, using a deterministic duration-based split:
- Training: 3,453 recordings, approximately 5 hours.
- Validation: 695 recordings, approximately 1 hour.
- Remaining recordings excluded from these subsets.

Audio was extracted and resampled to 22,050 Hz.
English transcripts were phonemized using `g2p-en`.

## Alignment experiment
A 20-record subset was tested with Montreal Forced Aligner
using the English US ARPA acoustic model and dictionary.
This is an experiment on a subset, not a full-corpus alignment.

## Current status
- Dataset preparation: completed.
- English phonemization: completed.
- Full-corpus forced alignment: pending.
- FastSpeech 2 feature preprocessing: not completed.
- FastSpeech 2 training and inference: not started.

## Configuration warning
The current YAML files are based on the upstream LJSpeech
configuration. They have not yet been fully adapted and validated
for Svarah. Do not treat them as a ready-to-run Svarah setup.

The original FastSpeech 2 preprocessing and alignment scripts
also require dataset-specific adaptation.
