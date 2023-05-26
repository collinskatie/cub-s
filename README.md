# CUB-S

We release `CUB-S`, a relabeling of a portion of the [`CUB`]() bird classification image test set with individual soft labels per concept group (e.g., over wing color, beak shape, etc). Each participant provided a soft label for all 28 concept groups, following the [original group listing](). The structure of this README is based on the form used by some of the same authors for [`CIFAR-10S` (individual soft labels over CIFAR-10 images)]([https://github.com/jcpeterson/cifar-10h/blob/master/README.md](https://github.com/cambridge-mlg/cifar-10s/tree/master/cifar10s_data))., which is based on the repository from [Peterson et al](https://github.com/jcpeterson/cifar-10h/blob/master/README.md).

## Repository Contents

* `cub-s_labels.json`: extracted soft labels per individual annotator, and per bird image and per concept. Parsing details below.
* `cub-s_human_data.csv`: de-anonymized lightly-processed annotation information collected during crowdsourcing on [Prolific](https://app.prolific.co/). [Pavlovia](https://pavlovia.org/) was used as a backend. Details on column information are included below. We make code available for the interface platform, UElic, available shortly. When uploaded, the interface code will be hosted [here](). 
* `construct_labels.ipynb`: example script to construct soft labels from elicited information. 
* `label_construction_utils.py`: helper functions to construct soft labels.
* We will include a custom dataloader shortly. For the time being, we recommend reading in the ``CIFAR-10`` test set without shuffling, and swapping in our labels for the corresponding examples (i.e., the example index key in the `json` files). 
* `process_data.ipynb`: notebook illustrating how raw data is parsed (converting `raw_human_data.csv` to `human_soft_label_data.json`).

## Mapping Soft Labels to CUB

As discussed in our [paper](https://arxiv.org/pdf/2207.00810.pdf), there are several forms of soft labels that can be constructed from our data (see Section 4.2 and Fig. 2). The label format that includes all information elicited from humans is: "Top 2, Clamp" (``T2 Clamp``). We recommend the use of this form of soft labels. 

`T2 Clamp` labels for each annotator are included in `cifar10s_t2clamp_redist10.json`, which is a dictionary where: 
* Keys are the example/image indexes (based on the ordered [CIFAR-10 test set](https://www.cs.toronto.edu/~kriz/cifar.html)).
* Values are annotators' processed soft label information for said example, in a list.
* Individual annotators' soft labels are included as lists, which can then be converted to numpy arrays and/or Tensors.

If you wish to apply the aggregation method used in the paper, you can take the mean of the labels per example. 

Note, as discussed in the paper, there is a free redistribution parameter (gamma) that determines how much mass should be spread over labels deemed possible, even if all mass has already been allocated. For instance, if an annotator allocates 80% probability to deer and 20% to horse, but doesn't select dog as impossible, this implies dog is also possible. How much mass should be allocated? We find in cross-validation that redist = 10% is best, so have used that here. If you would like to vary the redistribution amount, you may generate new labels with `construct_labels.ipynb`. 

If you wish to construct soft labels using a different label variety, you may run the `construct_labels.ipynb` script (see details in documentation). 

Alternatively, we provide a lightly processed version of the data elicited so you may create soft labels of any variety. This can be found in `human_soft_label_data.json`. The format follows `cifar10s_t2clamp_redist10.json`, but now, individual annotator's information itself is a dictionary, with keys: "Most Probable Class", "Most Probable Class Prob", "Second Most Probable Class", "Second Most Probable Class Prob", "Impossible Class(es)".

## Raw Data Format

The columns in our raw_data (`raw_cub-s_human_data.csv`) represent: 
time_elapsed,subject,concept_group,filename,evalAttrUncs,img_id
* subject: unique id randomly generated for a given annotator.
* response: annotations provided for a given image (most prob class w/ prob, second prob class w/ optional prob, any impossible classes). note, the final page shown to each annotator was a debrief questionarre; for this page, you can see comments to the questions included below. 
* img_id: integer into the original CIFAR-10 ordered test set for the image show.
* label: category assigned to the image according to the [CIFAR-10 test set](https://www.cs.toronto.edu/~kriz/cifar.html).
* filename: readable tag for image shown: "cifar10_train_{img_id}_{img_label}.png" note, these are from the "test" set. we called these "train" because we were training on the labels, but will soon change this tag, and downstream code which uses the filename, to avoid confusion. 
* rt: time spent (msec) on a given page, by an annotator.
* time_elapsed: total time (msec) an annotator has taken on the experiment so far.
* task: indicates the type of the screen shown to the annotator. "spec conf" is the soft label elicitation; "rerun_spec_conf" are repeat trials of earlier soft label elicitations (same screen type). 
* trial_index: the order of the trials/pages the annotator saw.
* condition: batch of images annotator was allocated to.
* trial_type: the [jsPsych](https://www.jspsych.org/6.3/) screen-type shown on a given page.
* view_history: meta-data on annotator viewing instructions.

## Citing

If you use our data, please consider the following bibtex entry: 

```
@inproceedings{collins2023humanConceptUnc,
      title={Human Uncertainty in Concept-Based AI Systems}, 
      author={Katherine M. Collins and Matthew Barker and Mateo Espinosa Zarlenga and Naveen Raman and Umang Bhatt and Mateja Jamnik and Ilia Sucholutsky and Adrian Weller and Krishnamurthy Dvijotham},
      year={2023},
      archivePrefix={AIES},
}
```

## Questions?

If you have any questions about `CUB-S` use, elicitation, and/or creation, please feel free to add a GitHub Issue and/or reach out to Katie Collins (`kmc61@cam.ac.uk`) and Matthew Barker (`mrlb3@cam.ac.uk`).

