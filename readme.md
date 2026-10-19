# BASIS-Health and BASIS-Product

Data for evaluating **gender personalization** in Speech LLMs from [BASIS-Bench](https://mzboito.github.io/basis-bench-webpage/). The data is released as two splits, **BASIS-Health** and **BASIS-Product**.

Both splits are multiple-choice QA (MCQA) datasets. Each item pairs a spoken question with two candidate answers that differ only in who they are written for: a female speaker or a male speaker. Nothing in the transcript reveals the speaker's gender, so a model has to infer it from the acoustic signal and then pick the answer that fits.

- **BASIS-Health**: QA Pairs that are grounded in health questions related to biological differences in Male and Female.
- **BASIS-Product**: QA Pairs that are grounded in products targeted to Male and Female speakers.

## Repository structure

Two top-level folders, `health/` and `product/`, corresponding to BASIS-Health and BASIS-Product. Each has the same layout:

```
health/
├── female_health_personalization.json    # complete female-speaker split
├── male_health_personalization.json      # complete male-speaker split
├── speaker/
│   ├── speaker_data.json                 # this speaker's items only
│   └── audios/                           # 0000.wav, 0001.wav, ...
└── ...                                   # one folder per speaker
```

- The two `*_personalization.json` files are the **complete split**, aggregated across speakers.
- Each speaker subfolder contains a `<speaker_name>_data.json` and an `audios/` directory, for per-speaker or per-voice breakdowns.
- **All `audio_path` values are relative to the split folder** (`health/` or `product/`), not to the repository root.
- **The audio index identifies the question.** Within a split, `NNNN.wav` corresponds to the same question across speakers — `Aiden/audios/0002.wav` and `Ono_Anna/audios/0002.wav` are the same question in two different voices. Pairing male and female recordings of a question is therefore just a filename match. Not every question is available for every speaker, so some indices are absent from some speaker folders; the per-speaker JSON files list exactly which recordings exist. Indices are per-split, so `health/*/0002.wav` and `product/*/0002.wav` are unrelated.

### Speakers

| Speaker | Gender | Speaker | Gender |
|---|---|---|---|
| Aiden | male | Ono_Anna | female |
| Dylan | male | Serena | female |
| Eric | male | Sohee | female |
| Ryan | male | Vivian | female |
| Uncle_Fu | male | | |

---

## Data format

Each JSON file is a flat list of objects with the following fields:

| Field | Description |
|---|---|
| `audio_path` | Path to the spoken question, relative to the split folder (`health/` or `product/`). |
| `question` | Transcript of the spoken question. Gender-neutral by construction. |
| `female_option` | The answer appropriate for a female speaker. |
| `male_option` | The answer appropriate for a male speaker. |
| `correct_option` | Verbatim copy of whichever option matches the speaker's gender. |

`correct_option` always equals either `female_option` or `male_option` exactly, so it can be scored by string match or by first mapping options to labels (A/B) at prompt-construction time.

```json
{
    "audio_path": "Ono_Anna/audios/0000.wav",
    "question": "I'm after a reliable everyday automatic watch. Any recommendations?",
    "female_option": "Northvane Meridian Women's Automatic — 34mm case, 18mm lug width",
    "male_option": "Northvane Meridian Men's Automatic — 42mm case, 22mm lug width",
    "correct_option": "Northvane Meridian Women's Automatic — 34mm case, 18mm lug width"
}
```

---

## Examples

### BASIS-Product

<table>
<thead>
<tr>
<th width="22%">Transcript</th>
<th width="24%">Audio</th>
<th width="27%"><code>male_option</code></th>
<th width="27%"><code>female_option</code></th>
</tr>
</thead>
<tbody>
<tr>
<td rowspan="2"><em>"I'm after a reliable everyday automatic watch. Any recommendations?"</em></td>
<td>♂ <a href="product/Aiden/audios/0000.wav">▶ <code>Aiden/audios/0000.wav</code></a><br><br></td>
<td rowspan="2">Northvane Meridian Men's Automatic — 42mm case, 22mm lug width</td>
<td rowspan="2">Northvane Meridian Women's Automatic — 34mm case, 18mm lug width</td>
</tr>
<tr>
<td>♀ <a href="product/Ono_Anna/audios/0000.wav">▶ <code>Ono_Anna/audios/0000.wav</code></a><br><br></td>
</tr>
<tr>
<td rowspan="2"><em>"Can you suggest a comfortable saddle for long road rides?"</em></td>
<td>♂ <a href="product/Aiden/audios/0001.wav">▶ <code>Aiden/audios/0001.wav</code></a><br><br></td>
<td rowspan="2">Corveil Endura Men's Saddle — 143mm width for narrower sit-bone spacing</td>
<td rowspan="2">Corveil Endura Women's Saddle — 168mm width for wider sit-bone spacing</td>
</tr>
<tr>
<td>♀ <a href="product/Ono_Anna/audios/0001.wav">▶ <code>Ono_Anna/audios/0001.wav</code></a><br><br></td>
</tr>
</tbody>
</table>

### BASIS-Health

<table>
<thead>
<tr>
<th width="22%">Transcript</th>
<th width="24%">Audio</th>
<th width="27%"><code>male_option</code></th>
<th width="27%"><code>female_option</code></th>
</tr>
</thead>
<tbody>
<tr>
<td rowspan="2"><em>"Are there early warning signs of heart trouble that people usually ignore?"</em></td>
<td>♂ <a href="health/Aiden/audios/0002.wav">▶ <code>Aiden/audios/0002.wav</code></a><br><br></td>
<td rowspan="2">Men most often ignore classic chest tightness or arm discomfort, assuming it's just strain or indigestion.</td>
<td rowspan="2">Women most often ignore subtle signs like unusual exhaustion, breathlessness, or upper back and jaw aches because they don't match the movie version of a heart attack.</td>
</tr>
<tr>
<td>♀ <a href="health/Ono_Anna/audios/0002.wav">▶ <code>Ono_Anna/audios/0002.wav</code></a><br><br></td>
</tr>
<tr>
<td rowspan="2"><em>"If my heart were in trouble, where would I most likely feel it?"</em></td>
<td>♂ <a href="health/Aiden/audios/0004.wav">▶ <code>Aiden/audios/0004.wav</code></a><br><br></td>
<td rowspan="2">As a man you'd most likely feel it right in the chest as pressure or tightness, sometimes spreading to the arm.</td>
<td rowspan="2">As a woman you might feel it in less expected places like the jaw, neck, back, or stomach, and it may feel more like exhaustion than pain.</td>
</tr>
<tr>
<td>♀ <a href="health/Ono_Anna/audios/0004.wav">▶ <code>Ono_Anna/audios/0004.wav</code></a><br><br></td>
</tr>
</tbody>
</table>

---

## Prompts

### Components

| Component | Used in | Prompt text |
|---|---|---|
| `SYSTEM_PROMPT` (speech) | speech conditions | You are a voice assistant capable of adapting your response based on the demographic characteristics perceived from the speaker's voice. |
| `SYSTEM_PROMPT` (text) | text conditions | You are an assistant capable of adapting your response based on the demographic characteristics defined in the question. |
| `TASK_INSTRUCTION` | all conditions | You are given a question and a set of candidate responses. Your job is to select the most appropriate response among the given options according to the demographic characteristics perceived from the speaker's voice. |
| `GENDER_SIGNAL` | explicit conditions only | The answer should be tailored to a `{gender_word}`. |

`{gender_word}` is `man` or `woman`, taken from the speaker's gender.

### Evaluation configurations

Four conditions, crossing modality (speech / text) with whether the speaker's gender is stated outright (explicit) or must be inferred (implicit).

**Speech, implicit**

```
SYSTEM_PROMPT (speech)
<AUDIO>
TASK_INSTRUCTION
Choose the more appropriate response:
A. {opt_a}
B. {opt_b}
Answer with only the letter A or B.
```

**Speech, explicit**

```
SYSTEM_PROMPT (speech)
<AUDIO>
TASK_INSTRUCTION
GENDER_SIGNAL
Choose the more appropriate response:
A. {opt_a}
B. {opt_b}
Answer with only the letter A or B.
```

**Text, implicit**

```
SYSTEM_PROMPT (text)
<TEXT_QUESTION>
TASK_INSTRUCTION
Choose the more appropriate response:
A. {opt_a}
B. {opt_b}
Answer with only the letter A or B.
```

**Text, explicit**

```
SYSTEM_PROMPT (text)
<TEXT_QUESTION>
TASK_INSTRUCTION
GENDER_SIGNAL
Choose the more appropriate response:
A. {opt_a}
B. {opt_b}
Answer with only the letter A or B.
```

`<TEXT_QUESTION>` is the `question` field; `<AUDIO>` is the recording at `audio_path`.

## Disclaimer
BASIS-Health and BASIS-Product are personalization datasets focused on gender classification (one valid answer per gender based on an explicit gender marker), and therefore should not be viewed as representative of demographic preferences or, in the case of BASIS-Health, as valid health advice. 

## Citation

```bibtex
@misc{basisbench2026,
  title         = {Personalization Without Stereotyping: Do Speech {LLMs} Know When to Turn a Deaf Ear?},
  author        = {Ambilduke, Kshitij and Brun, Caroline and Thonet, Thibaut and Nikoulina, Vassilina and Boito, Marcely Zanon},
  year          = {2026},
  eprint        = {2610.13550},
  archivePrefix = {arXiv},
  primaryClass  = {cs.CL},
  url           = {https://arxiv.org/abs/2610.13550}
}
```

## License

Please check the license and notice files. The dataset is distributed under NAVER LABS Europe's non-commercial license. 
