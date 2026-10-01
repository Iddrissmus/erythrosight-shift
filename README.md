# Blood Smear Classification Under Protocol and Site Shift

Does a deep learning classifier for haemoglobinopathies hold up when the blood sample
isn't imaged the way the training data was?

---

## The question

Most published red blood cell classifiers are trained and tested on images captured under
one consistent protocol: a controlled time between sample preparation and imaging, a
controlled temperature, one microscope, one site. The reported accuracies are high.

A district hospital in Ghana does not work that way. Samples wait. Rooms are hot. Equipment
and handling differ. If a model trained under protocol degrades badly outside it, then the
reported accuracy is not a measure of whether the tool works — it is a measure of whether
the laboratory conditions were met.

So, concretely:

> **How much does classification accuracy degrade as time-to-imaging and ambient temperature
> depart from the collection protocol, and how much does it degrade across collection sites —
> and is that degradation graceful or catastrophic?**

The *gap* is the result here, not the headline accuracy.

## Why this dataset

[erythroSight](https://www.frdr-dfdr.ca/repo/dataset/ca7ef0f8-28c2-4c3c-9f7c-3a30a8aac5f2)
(CC BY 4.0) contains over 300,000 microscopic blood cell images from 138 participants across
**two countries** — Mount Sagarmatha Polyclinic in Nepalgunj, Nepal, and BC Children's
Hospital and St Paul's Hospital in Vancouver, Canada. Participants fall into three groups:
sickle cell disease, beta-thalassemia, and controls with no known haemoglobinopathy.

Two things make it unusually well suited to the question above:

1. **Two collection sites** — a built-in site-shift experiment.
2. **Protocol variation** — most images were captured two hours after sample preparation at
   room temperature, but the dataset also includes other time points, other temperature
   settings, and time series data.

Both experiments are available without collecting any new data.

## Method

The order matters, and it is deliberately unglamorous.

1. **Inspect before modelling.** Participants per group, images per participant, distribution
   of time-to-imaging and temperature, class and site balance. Look at images from each class
   by eye until they can be told apart manually.
2. **Split by participant, never by image.** With ~300k images from 138 people, a random split
   leaks the same person into train and test and inflates accuracy into meaninglessness. Every
   split in this repository is at participant level.
3. **Establish a baseline.** A fine-tuned convolutional network on a balanced subsample,
   evaluated on a held-out set of participants. One honest number.
4. **Run the shift experiments.** Hold out an entire site; separately, hold out everything
   outside the two-hour room-temperature protocol. Report the degradation.
5. **Write up what failed.** Including the parts that did not work.

## Repository layout

```
notebooks/    exploratory and experimental notebooks
src/          reusable code — data loading, splits, training, evaluation
notes/        reading log: one note per paper, what it did and what I'd question
results/      metrics, figures, logs
data/         NOT tracked — see .gitignore
```

## Status

Setting up. Nothing is claimed here yet.

- [ ] Dataset access confirmed and subsample downloaded
- [ ] Participant-level split implemented and sanity-checked
- [ ] Exploratory analysis of class, site and protocol distribution
- [ ] Baseline classifier trained and evaluated
- [ ] Site-shift experiment
- [ ] Protocol-shift experiment (time and temperature)
- [ ] Write-up

## Reading log

Notes live in [`notes/`](notes/). Each entry: what problem the paper addressed, what was done,
what I would question.

## Author

Iddriss Hamidu Mustapha — BSc Computer Engineering, Kwame Nkrumah University of Science and
Technology, Ghana. Software engineer, moving into machine learning deliberately and in public.

## Licence and attribution

Code in this repository is MIT licensed. The erythroSight dataset is used under
CC BY 4.0 and is not redistributed here.
