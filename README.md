# Frequency and Relatedness Speech Transcription Program

A Jupyter notebook for scoring recorded trials from a spoken-production experiment. For each trial it runs offline speech recognition ([Vosk](https://alphacephei.com/vosk/)) with word timestamps, finds the speech onset, locates three target words in the transcript, and writes the timings to a results spreadsheet with order checks highlighted. Written in 2023. It measures timing only: the notebook does not compute word frequency or relatedness values.

## Inputs

```
Cache/
  <name>/            one folder per recording set
    <name>.xlsx      template: target words A, B, C (columns C-E) and target sentence (F), from row 3
    1.wav            one mono 16-bit WAV per trial, numbered in template row order
    2.wav
    ...
```

Plus the Vosk English model `vosk-model-en-us-0.22` from <https://alphacephei.com/vosk/models>. No audio or templates are included in this repository.

## Output

`<name>_Results.xlsx` in each subfolder: a copy of the template with, per trial:

| Column | Content |
|---|---|
| G | Recognized words (`c` if the transcript matches the target sentence exactly) |
| H | `1` if the transcript matches the target sentence |
| I | Speech onset (ms) |
| J, K, L | Start time (ms) of target words A, B, C |
| M onward | Silent gaps as begin/end pairs (ms) |

Target words also match common misrecognitions listed in the notebook's `misidentified_words` dictionary, plurals, and one-character differences. Cells are highlighted yellow when the gap from speech onset to word A is under 25 ms or over 300 ms, and red when the onset and word times are out of order.

## Setup and usage

```
pip install -r requirements.txt
```

1. Download and unzip the Vosk model.
2. Open the notebook in Jupyter (Python 3.9).
3. In the processing cell (cell 2, or cell 3, an alternative with a different onset detector), set `dir_path` to the folder holding the `<name>` subfolders and `model_path` to the Vosk model folder.
4. Run it. Every subfolder is processed and its results file overwritten.

Cells 4 and 5 delete files (duplicate `(1).wav` downloads and old `_Results.xlsx` files) without asking. Run them only on purpose.

## Limitations

- Paths are hard-coded Windows paths; edit them for your machine.
- The audio sample rate is not checked; the silence window assumes 24 kHz.
- Word matching depends on the Vosk model and on the notebook's hand-written alternates.
- Not packaged or tested as a library. No accuracy results are reported.
