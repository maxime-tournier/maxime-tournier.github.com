---
title: Vicon Nexus Workflow
---

A few notes on using Vicon Nexus for biomechanics.

# Good Practice

- 1 session per marker-set: if marker arrached/dropped, use another
  session
- stick to the standard plugin-gait marker set as much as possible
  (TODO marker image)
- don't forget to write down the subject mass in session/subject
  notes, or read it from the force plates if available: you'll need it
  later!
- you may skip redundant markers (e.g LUPA/RUPA/RBAK/RFRM/LFRM...) but
  make sure you have all the joint markers

# Subject Preparation

1. pick the session where you have the cleanest set of markers
2. create a new subject from the full-body plugin-gait template
3. if you lack markers, detach them from the marker set in the subject
   you just created
   - usual suspects include LTIB, RTIB, RFRM, LFRM and RBAK, maybe T10
     if you use a backpack
4. enter subject mass in the subject properties
5. don't forget to save your subject!

# Subject Calibration

1. pick the cleanest trial: ideally it should have something like a
   t-pose, or close, then find the best frame.
2. you might want to try auto-labeling here, but it never really worked for me
3. if auto-labeling fails, manually label the markers in this frame
4. select the "plugin-gait" static pipeline, check the "processing
   static subject calibration", and run it
5. save your subject!

# Auto-Labeling

1. in the data-management plane, mark all the trials in this session
2. in the batch processing toolbox, run the reconstruct and label
3. you may also want to run the "auto-intelligent gap fill" if your
   captures were a bit messy
   
# Region of Interest

1. for each capture, adjust the region of interest, right click and
   "zoom to region of interest"
2. save the trial
3. cleanup the remaining artifacts (fill gaps, etc...)

# Other Sessions

note: this assumes a (reasonable) subset of the markers used in the
cleanest session

1. load the subject file from the previous (cleanest) session
2. save the subject
3. select all the trials in this session
4. run the "reconstruct and label", then "auto-intelligent gap-fil"


# Missing Markers

- if some markers were absent/invisible during a trial, you may get
  away by running the 'kinematic fit' pipeline (but it's unclear how
  to export the computed trajectories if there where no holes)

# I MESSED UP! WAT DO?!?!!

- right-click on the timeline, zoom-to-trial, reset start/end time,
  then run reconstruct or recontruct/label
