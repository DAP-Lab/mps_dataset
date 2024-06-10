# Maharashtra Primary Schools (MPS) Dataset

This dataset has been released as part of the Interspeech 2024 research paper titled "A Dataset and Two-pass System for Reading Miscue Detection". 
If you find the MPS dataset useful in your research, kindly use the following BibTeX entry for citation:
`Citation details to be added here`

## Dataset details
This dataset contains audio recordings and text transcripts of 1600 utterances of read aloud text across 1110 unique speakers. These audios were collected as part of a benchmarking exercise in the summer of 2023 for testing reading levels in Grades 3, 4 & 5 (age 7-11 years) of 10 government schools across the Indian states of Maharashtra and Goa on L2 English grade-appropriate texts. The [text prompts](#text-prompts) which comprise of the 2 paragraphs of a single story, each between 60-80 words, with a unique story assigned to each grade have also been provided. The total audio duration of the dataset is 19 hours with speakers uniformly distributed across the three grades. The dataset is fully labeled with filler/non-speech events and word-level transcriptions using a [semi-automated transcription process](#manual-transcription-process). The reading errors are observed to comprise a number of non-English words, which are transcribed phonemically and a lexicon with the [phone sequences](#phone-set-used-in-lexicon) and alternate pronunciation for all words used in transcription has been made available. Additionally, the school grade and gender information of the speakers is provided as metadata. Next, the miscue labels of transcript words with reference to aligned text prompts post the [alignment process](#alignment-process) are made available as Cor/Subs/Ins/Del. From the word-level transcript, the error type (e.g. partial word) can further be inferred for any studies requiring this. 

Ethics clearance was obtained for the audio recording with anonymised speaker information but for grade and gender. The students, who come with diverse home languages, are introduced to both Hindi and English reading and writing in Grade 1. Unlike L1 English learners of reading, they have a non-existent (or very limited) vocabulary for English, and are therefore encountering new words and their written forms simultaneously. They cannot draw on any aural memory to sound the written form and hence make a variety of errors in pronunciation owing to the opaque orthography of English as well as phonotactic constraints of their home language. 

## Text Prompts
The grade-appropriate text passages were chosen from the [English 400 Reading Programme](https://www.orientblackswan.com/books?id=0&pid=0&sid=40) from [EFLU](https://www.efluniversity.ac.in/) which contain 500 reading cards arranged in 10 levels of difficulty starting from 1 and going all the way until 10. Within each level, there are 5 categories containing 10 cards each with increasing difficulties starting from Card 1-10 (easiest) to Card 41-50 (most challenging). The programme recommends the following levels for reading:
- Level 1/2 : Beginners
- Level 3/4: Someone who has studied English for 2-3 years.

The passages chosen for the 3 Grades are given below. Note that each story has been split up into 2 paragraphs of 50-75 words each.

### Grade 3

**The Lost Sheep** (125 words, Level 2 Card 34)

Once, there lived a shepherd in a village. He was a kind and good man. He had a hundred sheep. One day, he took the sheep to the riverside. There was a lot of grass there for the sheep. In the evening, the shepherd took them back home. He counted the sheep. There were only ninety-nine! One sheep was lost. The shepherd was sad.

He went out looking for the lost sheep. He walked all the way back to the river. At last, late in the night, he found the sheep in a bush. The bush was full of thorns. He got the sheep out and carried it home. He was happy. He showed the sheep to his friends. All of them were happy too.

### Grade 4

**Bears** (139 words, Level 4 Card 26)

Bears are found in Europe, Asia, Africa and America. They have big bodies, with short tails and thick legs. Polar bears, which live in the coldest part of the world, usually eat fish and seals. When kept in cages or in zoos, bears like to eat meat, vegetables, milk and rice. Bears are not quite as dangerous as we think they are. Like most animals, they will do their best to keep away from us.

In the colder parts of the world, bears hibernate from October to April. Before hibernating, they eat a lot of food and become fat. Although bears have very poor eyesight and hearing, they can smell very well and are very clever. At the zoo, you may have noticed how cleverly they beg for biscuits and nuts, by sitting up and holding out their paws.

### Grade 5

**Butterflies** (149 words, Level 5 Card 38)

Butterflies are surely some of the most beautiful creatures in the world. They are beautiful because of the brilliant colours of their wings. If you look at a butterfly’s wings through a magnifying glass, you will see that they are covered with very small scales. It is these scales which have colour. Without these scales, the wings would be transparent. The colours on the wings make very fine patterns.

If you look at pictures of butterflies, you will find that many of them have large, black circles on their wings. These look like eyes. These ‘eyes’ are very useful to the butterfly. They help it to escape from its enemies. A bird or a lizard which is trying to catch a butterfly mistakes these black circles for the creature’s eyes. It aims or tries to hit the ‘eye’. But all that it gets is a bit of the wing.

## Manual transcription process

The procedure for manual transcription is as follows:

The collected audio recordings are first passed through an in-house ASR [Kaldi TDNN with Language Model trained on the text prompts and a garbage model] to get the decoded text. Using the alignment of the decoded text with the text prompts and further using the time-stamps, sentence-level transcripts are created in an Audacity label track format. The audio and the label track are then opened in Audacity by the transcriber for sentence level transcription. The transcribers are expected to label each word based on what they perceive as uttered in the audio. They type in English for words from the English language and use Devanagari (phonetic alphabet easily convertible to phone symbols) for intelligible but invalid English words. Devanagari transcription typing is done using the Microsoft’s Indic Language Input Tool (ILIT) installed on the transcriber’s machine. Additionally, the transcriber is trained to use the following labels for certain common events encountered in the recordings. 

| Transcription Label | Description |
| --- | --- |
| SIL  | Silence (>200ms)  |
| BR  | Breath (inhalation/exhalation), sniffling |
| ON  | Other noise appearing in isolation e.g. Noise of birds, mobile, vehicles, bell, mic noise, clearing of throat   |
| FP  | Filled pauses like uh, hmm, umm, etc.  |
| IR  | Irrelevant speech (background speaker) present at the start or end of the recording   |
| MB  | Unintelligible words or mumbling  |
| WH  | Child whispering a word (many a times this can be heard when the child is trying to spell out the word)  |

The completed transcriptions are further QC’d by an expert comfortable with both English and Devanagari and aware of the labelling conventions.

## Alignment process
Ground-truth miscue labels (Cor/Sub/Del/Ins) derived by comparison of the manual transcription with suitably aligned text prompts (using a phonetically oriented alignment)

For the reading assessment task, the alignment of the
prompt words with the ASR hypothesis words (or with the manual transcript) needs to be more nuanced than that achievable
with conventional word-level edit distance, i.e. where exact
match is considered and errors comprise substitutions, deletions, or insertions, based on the Levenshtein distance. In order to predict the precise types of reading miscues accurately
and also obtain the correct word boundaries for the subsequent
stage of local feature computation, the ASR output sequence of
words needs to be aligned with the reading prompt sequence of
words in a manner that is phonetically informed. This exploits
the expected phonetic similarity between the uttered word and
attempted prompt word. We modify the alignment algorithm of  [POWER](https://github.com/NickRuiz/power-asr) to accommodate the peculiarities of the reading
application such as the occurrence of word splitting or merging.
False starts, a common event in oral reading, are accommodated
with bias towards right alignment. We also account for the possibly multiple valid pronunciations of a prompt word.

Please scroll right in below panel to see the entire aligned text for the utterance ([audio](audios/4a42f_EN-OL-RC-426_2.wav)).  The symbol `<eps>` indicates  Insertion/Deletion.

```python

Prompt-Manual transcription Alignment:
#Correct (c): 47 , #Substitution (s): 15, #Insertion (i):4, #Deletion (d):2

Prompt               :  in     the     colder      parts     of     the     world     bears     hibernate       from     october       to     april     before     hibernating     they      eat     a     lot     of     food     and     become     fat     although     <eps>     bears     have     very     <eps>     poor     eyesight     and     hearing     they     can     smell     very     well     and     are     very     clever     at     the     zoo       you     may      have     noticed     how     <eps>     cleverly     they     beg     <eps>     for     biscuits     and     nuts     by     sitting     up     and     holding     out     their     paws      
Manual transcription :  in     the     coldest     part      of     the     world     beers     हायब्रेटिंग         from     औक्टोम्बर       to     april     before     हायब्रेटिंग          there     eat     a     lot     of     food     and     become     fat     अल_throw    bee       beers     have     very     पोला       poor     eye_side     and     hearing     they     can     smell     very     well     and     are     very     क्लेअर      at     the     <eps>     you     many     have     noticed     how     कि        clearly      they     beg     MB        for     biscuit      and     nuts     by     sitting     up     and     holding     out     they      <eps>     
Miscue Label         :  c      c       s           s         c      c       c         s         s               c        s             c      c         c          s               s         c       c     c       c      c        c       c          c       s            i         s         c        c        i         c        s            c       c           c        c       c         c        c        c       c       c        s          c      c       d         c       s        c        c           c       i         s            c        c       i         c       s            c       c        c      c           c      c       c           c       s         d         
```

## Phone Set used in Lexicon

# List of phones used in this work with sample English word

| Phone | Sample Word    | &nbsp;&nbsp;&nbsp;&nbsp; | Phone | Sample Word |
| --- | --- | --- | --- | --- |
| AA | Bath    | &nbsp;&nbsp;&nbsp;&nbsp; | K | back |
| AE | Hand    | &nbsp;&nbsp;&nbsp;&nbsp; | L | lost |
| AH | world   | &nbsp;&nbsp;&nbsp;&nbsp; | M | meat |
| AO | Log     | &nbsp;&nbsp;&nbsp;&nbsp; | N | can |
| AW | Mouse   | &nbsp;&nbsp;&nbsp;&nbsp; | NG | sitting |
| AY | Like    | &nbsp;&nbsp;&nbsp;&nbsp; | OW | polar |
| B | Web      | &nbsp;&nbsp;&nbsp;&nbsp; | OY | boy |
| CH | pinch   | &nbsp;&nbsp;&nbsp;&nbsp; | P | up |
| D | would    | &nbsp;&nbsp;&nbsp;&nbsp; | R | colour |
| DH | although| &nbsp;&nbsp;&nbsp;&nbsp; | S | circles |
| EH | went    | &nbsp;&nbsp;&nbsp;&nbsp; | SH | asia |
| ER | herd    | &nbsp;&nbsp;&nbsp;&nbsp; | T | tries |
| EY | way     | &nbsp;&nbsp;&nbsp;&nbsp; | TH | thorns |
| F | cough    | &nbsp;&nbsp;&nbsp;&nbsp; | UH | bush |
| G | plug     | &nbsp;&nbsp;&nbsp;&nbsp; | UW | zoo |
| HH | had     | &nbsp;&nbsp;&nbsp;&nbsp; | V | vote |
| IH | his     | &nbsp;&nbsp;&nbsp;&nbsp; | W | walk |
| IY | sheep   | &nbsp;&nbsp;&nbsp;&nbsp; | Y | yard |
| JH | village | &nbsp;&nbsp;&nbsp;&nbsp; | Z | was |
| | | &nbsp;&nbsp;&nbsp;&nbsp; | ZH | visual |


## Contents of the repository
- `audios` folder contains the oral reading attempts of the students captured at sampling rate of 16kHz and stored as 16-bit mono PCM (.wav) files. The background noise varies from barely audible to classroom noise in the vicinity of the speaker.\
- `data.json` is a common file containing the metadata associated with each attempt using human-readable fields. For further explanation, one of the entries from the array has been described in detail [here](#json-example-with-details)
- `lexicon_IS2024.txt` contains all the words and the corresponding phone sequence with alternative pronunciations used in transcription.\
- `notebook.ipynb` explains how to read a data.json file and extract the required information.

For further details, check out the following link: [Supplementary material](https://lapis-homegrown-710.notion.site/Supplementary-material-Interspeech-2024-submission-810faeb994bd4607aafdb7b12b730f55)

## JSON example with details
```
{
    "audioID": "5d44c_EN-OL-RC-538_2",  // Unique Audio File Identifier
    "audioPath": "audios/5d44c_EN-OL-RC-538_2.wav", // Audio File path
    "metaData": {                                   // Speaker Metadata
        "speakerID": "5d44c",                       // Speaker ID
        "gender": "Male",                           // Speaker Gender
        "grade": "5",                               // Speaker Grade
        "storyID": "EN-OL-RC-538",                  // Speaker Prompt ID
        "paragraphID": "2"                          // Speaker Prompt ID paragraph
    },
    "manualTranscript": " SIL if you look at the pictures SIL of butterflies you will SIL find that many of them have large black circles on their wings SIL this look likes eyes these eyes BR are very useful ON to the butterfly SIL they help SIL it to escape from its enemies a bird SIL of a लिजा्ड SIL which is trying to catch SIL a butterfly mistakes the these black circles ON for the क्रे क्रेचर ON eyes it aims to ties SIL to hit the eye but all that SIL he it gets it SIL a bit of the wings SIL",                                               // Manual Transcription
    "promptText": "If you look at pictures of butterflies, you will find that many of them have large, black circles on their wings. These look like eyes. These eyes are very useful to the butterfly. They help it to escape from its enemies. A bird or a lizard which is trying to catch a butterfly mistakes these black circles for the creature's eyes. It aims or tries to hit the eye. But all that it gets is a bit of the wing.",  // Prompt Text containing punctuation
    "textAlignment": [                              // Alignment results between prompt text and manual transcription
        {
            "promptTextWord": "if",               
            "manualTranscriptWord": "if",
            "miscueLabel": "c"                       // c - Correct, i - Insertion, d - Deletion, s - substitution
        },
        {
            "promptTextWord": "you",
            "manualTranscriptWord": "you",
            "miscueLabel": "c"
        },
        {
            "promptTextWord": "look",
            "manualTranscriptWord": "look",
            "miscueLabel": "c"
        },
        {
            "promptTextWord": "at",
            "manualTranscriptWord": "at",
            "miscueLabel": "c"
        },
        {
            "promptTextWord": "<eps>",              // <eps> - Symbol used for insertions/deletion cases
            "manualTranscriptWord": "the",
            "miscueLabel": "i"
        },
        ...
    ]
}
```
