# ConvoAAVE

**An African American Vernacular English speech corpus, built so that speech recognition systems stop failing the people who speak it.**

A project of [Machina Mundi](https://machinamundi.vercel.app) — Project POLLEN (Preserving Oral Language & Linguistic Equity Now).

> *"When our children ask a voice assistant a question, they deserve to be understood."* — Hartford community member

---

## The problem

Automatic speech recognition does not work equally well for everyone. Published audits of the major commercial ASR systems have found substantially higher word error rates for Black speakers than for white speakers reading or saying comparable material. The gap is not evenly distributed across the language — it concentrates on exactly the features that distinguish African American Vernacular English.

The cause is representation, not intent. Speech models learn from audio, and the audio they are trained on underrepresents AAVE speakers. A system that has rarely encountered habitual *be*, stressed *been*, or *finna* does not recognize them as grammar. It treats them as noise and normalizes them into something the speaker did not say.

That failure is now load-bearing. Voice assistants, automatic captioning, medical and legal dictation, voice search, and classroom transcription tools all sit on top of ASR. When the transcript is wrong, everything downstream of it is wrong too.

## What ConvoAAVE is

ConvoAAVE is a speech corpus: conversations and first-person narratives recorded with community members in Hartford, Philadelphia, and Los Angeles, then transcribed to preserve AAVE grammar rather than normalize it away.

**The audio is not published, and will not be.** A voice identifies a person. These recordings are named community members talking about their own lives, their families, and their neighborhoods. Releasing the audio would expose the people who agreed to be recorded in order to make the corpus more convenient for researchers. That trade is not ours to make.

What this repository publishes is the **transcript layer**, plus the methodology behind it.

## What is in this repository

| Path | Contents |
| --- | --- |
| `data/compiledFINAL.txt` | The main transcript file. ~83,800 words. Two-speaker conversational exchanges (marked `A:` / `B:`), extended first-person narrative speech, and a systematic set of AAVE grammatical constructions. |
| `data/organized/reddit_aave.txt` | **Supplementary written text**, not speech. ~3,300 words collected from public Reddit posts and comments. |
| `data/organized/tweets_aave.txt` | **Supplementary written text**, not speech. ~2,200 words collected from public tweets. |
| `scripts/reddit_scrape.py` | Collection script for the Reddit supplement. |
| `scripts/x_scraping.py` | Collection script for the Twitter/X supplement. |
| `repoArchitecture.perl` | Repository layout helper. |

The two social-media files are kept deliberately separate and clearly labeled. They are **written** language, gathered from public posts, and they are not part of the speech corpus. Mixing scraped text into a speech dataset without labeling it would make the corpus useless for the thing it exists to measure.

### Construction coverage

Alongside the conversational material, the corpus systematically covers AAVE morphosyntax, so a model can be evaluated construction by construction instead of scored on one blended number:

| Construction | Example |
| --- | --- |
| Habitual *be* | "He be cleaning his truck Fridays" |
| Stressed *been* | "She been had that job since May" |
| Completive *done* | "We done finished everything already" |
| *finna* | "She finna wash her car" |
| *stay* + V-ing | "She stay forgetting names" |
| Negative concord | "You ain't got no jacket" |
| *might could* | "We might should leave soon" |
| Zero copula | "He look like he confused" |

This matters because a single aggregate error rate hides the failure. A system can post a respectable overall number while getting habitual *be* wrong nearly every time.

## How it was collected

1. **Community partnerships.** Churches, schools, community centers, and family networks were approached first, and the project was explained before anything was recorded.
2. **Consent.** Participation was voluntary and revocable. Speakers were told what the recording was for and who would be able to hear it.
3. **Natural speech, not read prompts.** Unscripted conversation and extended storytelling, so the corpus carries real rhythm, overlap, and discourse structure.
4. **Transcription as spoken.** Transcribed to preserve AAVE grammar. A transcript that "corrects" the speaker destroys the exact signal the corpus exists to provide.
5. **Restricted audio.** Recordings are held under restricted access. Transcripts and methodology are public.

## How ConvoAAVE differs from existing resources

ConvoAAVE is not the first AAVE speech resource, and it does not need to be. [CORAAL](https://oraal.uoregon.edu/coraal) (the Corpus of Regional African American Language) is the established reference corpus and the foundation this kind of work builds on.

ConvoAAVE is built differently in three ways:

- **Privacy-preserving by design.** Transcripts are public; audio is not. Most speech corpora make the opposite trade.
- **Community-governed.** Collected through community institutions, with participants retaining the right to withdraw.
- **Evaluation-oriented.** Paired with explicit construction coverage, so it functions as a diagnostic set and not only as training material.

## Using it

The transcripts are plain UTF-8 text, one utterance or turn per line. Conversational turns in `compiledFINAL.txt` are prefixed `A:` / `B:`; blank lines separate conversations.

```bash
git clone https://github.com/karthiksrikumar/ConvoAAVE-POLLEN.git
cd ConvoAAVE-POLLEN

# word count of the transcript layer  -> 83796
wc -w data/compiledFINAL.txt

# conversational turns  -> 856
grep -cE "^[AB]:" data/compiledFINAL.txt

# habitual be + V-ing  -> 472 examples
grep -inE "\bbe [a-z]+in[g']?\b" data/compiledFINAL.txt | head -40
```

If you want to evaluate an ASR or language model against the construction set, filter `compiledFINAL.txt` to the single-clause example lines and score per construction rather than in aggregate.

## Status and roadmap

This is an active project, not a finished release.

- **Now:** transcript layer published; audio under restricted access; construction coverage assembled.
- **In progress:** speaker and regional metadata for the transcripts; a documented per-construction evaluation harness.
- **2026:** partnerships with Historically Black Colleges and Universities to expand regional coverage; a formal data statement and datasheet.

Evaluation results are not published here yet. When we have numbers we can stand behind, with the code that produced them, they will go in this repository alongside the corpus.

## Requesting audio access

Researchers who need the audio for legitimate speech work can contact Machina Mundi to discuss restricted access under terms that protect participants. Access requires an agreement covering non-redistribution and non-identification.

## Ethics

- **Consent** — community-approved collection, revocable by the participant.
- **Attribution** — credit to the language community, not extraction from it.
- **Ownership** — community control over how the data is used.
- **Benefit-sharing** — improvements return to the classrooms and families that built the corpus.
- **Transparency** — methodology and limitations documented, including what this corpus is *not*.

### Known limitations

Stated plainly, because a dataset used for evaluation has to be honest about its own edges:

- Regional coverage is uneven, weighted toward Hartford.
- Speaker-level demographic and regional metadata is incomplete.
- The construction examples are constructed illustrations, not spontaneous speech, and should be treated as a diagnostic set rather than as naturalistic data.
- The two social-media files are written text and are not representative of speech.
- The corpus is small relative to the commercial datasets it is meant to critique.

## License

MIT for the code and the released transcripts — see [LICENSE](LICENSE). Audio is not covered by this license and is not distributed. Please cite Machina Mundi if you use the corpus.

## Acknowledgments

To the community members who trusted us with their words; to the partner churches, schools, and community centers in Hartford, Philadelphia, and Los Angeles; and to the sociolinguists whose work on AAVE made it obvious that the dialect has grammar worth getting right.

---

*Machina Mundi — Innovate. Empower. Elevate.*
