# EEG Lie Detection Dataset using Emotiv EPOC+: Instructed-Response and Free-Response Protocols

EEG recordings from 54 participants who answered 32 yes/no questions while wearing a 14-channel Emotiv EPOC+ headset (128 Hz). Each answer is labelled as truthful or deceptive. Two protocols were run with the same questions, each with a different group of participants:

| Protocol | Task label | Participants | Where the truth/lie label comes from |
|---|---|---|---|
| Instructed response | `task-instructed` | 32 (`sub-I01` to `sub-I32`) | Assigned in advance: participants were told which blocks to answer truthfully and which deceptively |
| Free response | `task-free` | 22 (`sub-F01` to `sub-F22`) | Chosen by the participant on every question and ticked as Truth or Lie on a paper answer form |

In total there are 1,728 recordings (one per question per participant): 1,024 in the instructed protocol (512 truth, 512 lie) and 704 in the free protocol (409 truth, 295 lie). The instructed protocol is balanced for every participant (16 truth, 16 lie). In the free protocol the balance differs between participants, because participants chose for themselves.

## Authors

Md. Ashikur Rahman\*, Md Al Nomun, Rizu Ahmed, Abu Saleh Md Seam, Nazmus Sakib, Md. Kafiul Islam\*

Department of Electrical and Electronic Engineering (EEE), Independent University, Bangladesh (IUB), Dhaka, Bangladesh. \*Corresponding authors.

## Funding

No funding . Supported by the Department of Electrical and Electronic Engineering (EEE), Independent University, Bangladesh.

## Participants

The participants were student volunteers from Independent University, Bangladesh. The two protocols were run with different groups of people. Age and sex are in `participants.tsv`.

| Group | n | Age in years, mean (SD) | Age range | Male / Female |
|---|---|---|---|---|
| Instructed protocol | 32 | 25.2 (2.4) | 21 to 30 | 20 / 12 |
| Free protocol | 22 | 25.0 (1.9) | 21 to 28 | 17 / 5 |
| All | 54 | 25.1 (2.2) | 21 to 30 | 37 / 17 |

Handedness and other background information were not recorded for this dataset.

## Experimental design

Every participant answered the same 32 questions, in four blocks of eight, always in this order: (1) personal information, (2) general knowledge, (3) control questions, (4) situation-based questions. The question list is given in `question_text` in each `events.tsv`.

In the instructed protocol, blocks 1 and 3 (personal information, control) were to be answered deceptively and blocks 2 and 4 (general knowledge, situation-based) truthfully. Blocks were separated by rest breaks of about two minutes, and each answer was followed by a rest of about ten seconds. A full session took about 15 to 20 minutes per participant.

Recordings were made in the BISPL lab at Independent University, Bangladesh (Dhaka), a compact room that students use to collect biomedical data. Participants sat on a chair in the room. The questions were read aloud in English by an interviewer, and participants answered verbally with yes or no. In the free protocol, participants also ticked Truth or Lie on a paper answer form after each answered question. Before every session the signal quality in EmotivPRO (EEG quality and contact quality) was checked.

Sessions were conducted by Md. Ashikur Rahman, Md Al Nomun, Abu Saleh Md Seam and Rizu Ahmed under the supervision of Md. Kafiul Islam. Instructed-protocol sessions took place between 30 August and 24 September 2025 and free-protocol sessions between 3 and 11 December 2025. All recordings used the Emotiv EPOC+ headset with saline sensors and the EmotivPRO software.

## Dataset organisation

- `sub-<label>/eeg/` holds one recording per question: `sub-<label>_task-<task>_run-<NN>_eeg.edf` with matching `_events.tsv` and `_channels.tsv`. The run number is the question number (1 to 32).
- `sub-<label>/sub-<label>_scans.tsv` lists the recordings. 
- `task-<task>_eeg.json` and `task-<task>_events.json` describe the recordings and the event columns.
- `participants.tsv` lists the participants with age, sex and trial counts. `source_folder` and `source_session_number` link each participant to the original recordings: in both protocols they equal the participant number in the `sub-` label (for example `sub-F18` is `Subject18`).
- `code/` contains the conversion script and `bids_mapping.tsv`, which maps every EDF file to the original EmotivPRO CSV file.

Each `events.tsv` has a single row covering the whole recording, with the label (`trial_type`: `truth` or `lie`), question number, question category, block, question text and `label_source` (`assigned` or `self_report`).

## Signal format

- 14 EEG channels: AF3, F7, F3, FC5, T7, P7, O1, O2, P8, T8, FC6, F4, F8, AF4, in microvolts, sampled at 128 Hz. The reference is the headset's fixed CMS/DRL reference.
- The signals are the raw EmotivPRO output. They have not been filtered or had the DC offset (around 4200 µV) removed.
- The recordings are released untrimmed. For the associated project, a window of 5 seconds was taken from each recording for analysis; the complete recordings are provided here.
- The EDF files were converted from the original CSV exports. EDF stores 16-bit integers, so values can differ from the CSV by at most 0.065 µV.
- Recording lengths vary between about 5.9 and 11.4 seconds (753 to 1,457 samples). EDF requires whole seconds, so in all but four files the last second is padded by repeating the final sample, and the padded part is marked with an EDF annotation called "Padding (repeats last sample)". The true signal length is the `duration` in `events.tsv`. Please cut the signal to that length before analysis.
- The original export also contains motion, facial-expression, mental-command, performance-metric and band-power columns. They are not in the EDF files; the original CSV for each recording can be found through `code/bids_mapping.tsv`.

## Points to be aware of

- Label and question category are linked in the instructed protocol. Because the truth/lie condition was fixed per block and the blocks were asked in a fixed order, label, question category and position in the session are confounded. A classifier trained on this protocol alone can learn question type or time in session rather than deception. The free protocol, where participants chose per question, does not have this fixed link.
- Free-protocol labels are self-reported and were not independently verified.

## Ethics

The study was approved by the Institutional Review Board of Independent University, Bangladesh (project number 2025-SETS-NSR:04). The board gave verbal approval in June 2025, before data collection started, and issued the written certificate of approval on 4 September 2025. All participants gave written informed consent before taking part. Participants are identified only by anonymous labels. Names, student IDs and contact details that were written on paper forms during data collection are not part of this dataset. Consent for sharing the de-identified data publicly was obtained from the participants, and the IRB confirmed that the data may be shared publicly.

## License

CC0 (public domain dedication).

## Contact

Authors: Md. Ashikur Rahman (ar.ashikurrahman27@gmail.com) , Md Al Nomun (alnoman3940@gmail.com) , Rizu Ahmed (rizuahmedsahil15@gmail.com) , Abu Saleh MD Seam (mdseam123456@gmail.com) , Nazmus Sakib(n.sakib@iub.edu.bd) and Md. Kafiul Islam (kafiul_islam@iub.edu.bd).
