# Telugu FastSpeech 2 Adaptation

## Dataset
SPRINGLab IndicTTS Telugu:
- Training: 1,168 recordings, approximately 5 hours.
- Validation: 199 recordings, approximately 1 hour.
- Remaining recordings excluded from these subsets.

Audio was extracted and resampled to 22,050 Hz.
Transcript text was retained in Unicode NFC form.

## Text representation
A Unicode character-level vocabulary was created:
- 71 observed characters.
- Padding and unknown tokens.
- Total vocabulary size: 73.

The Telugu dataset loader was modified to load the nested
`char_to_id` vocabulary and encode Telugu text by character.
The original English text encoding path is retained.

## Alignment experiment
An XLS-R Telugu CTC model was used for character-level alignment
experiments. Alignment spans were structurally validated against
the reference transcripts.

These CTC spans are not equivalent to verified phoneme-level
Montreal Forced Aligner TextGrids. Duration candidates derived
from their boundaries are experimental and have not been
accepted as final FastSpeech 2 duration labels.

Some records remain flagged for manual review.

## Current status
- Dataset preparation: completed.
- Unicode tokenizer: implemented and tested.
- CTC character alignment experiment: completed.
- Reliable duration labels: pending.
- FastSpeech 2 feature preprocessing: not completed.
- FastSpeech 2 training and inference: not started.

## Configuration warning
The current YAML files are experimental. Their paths must be
adapted to the execution environment. The original preprocessing
and alignment entry points have not yet been adapted to
IndicTTS Telugu.
