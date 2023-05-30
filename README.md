# CUB-S

We release `CUB-S`, a relabeling of a portion of the [`CUB`](https://www.vision.caltech.edu/datasets/cub_200_2011/) bird classification image test set with individual soft labels per concept group (e.g., over wing color, beak shape, etc) as part of our upcoming [AIES paper](https://arxiv.org/pdf/2303.12872.pdf). Each participant provided a soft label for all 28 concept groups, following the [original group listing](https://worksheets.codalab.org/rest/bundles/0xd013a7ba2e88481bbc07e787f73109f5/contents/blob/attributes/attributes.txt). The structure of this README is based on a doc from some of the same authors for [`CIFAR-10S`](https://github.com/cambridge-mlg/cifar-10s/tree/master/cifar10s_data) (individual soft labels over CIFAR-10 images), which is itself based on the repository from [Peterson et al](https://github.com/jcpeterson/cifar-10h/blob/master/README.md).

More details on our work can be found at our [project page](https://sites.google.com/view/human-concept-uncertainty?usp=sharing). 

## Repository Contents

* `cub-s_labels.json`: extracted soft labels per individual annotator, and per bird image and per concept. Parsing details below.
* `cub-s_human_data.csv`: de-anonymized lightly-processed annotation information collected during crowdsourcing on [Prolific](https://app.prolific.co/). [Pavlovia](https://pavlovia.org/) was used as a backend. Details on column information are included below. We make code available for the interface platform, UElic, available shortly. When uploaded, the interface code will be hosted [here](https://github.com/collinskatie/u-elic). 

## Mapping Soft Labels to CUB

*A cleaned script highlighting data loading will be released shortly*; if you need access sooner, please reach out to the authors (see Contact below). For now, as a start, we include details on the `CUB-S` soft labels below. We encourage downloading and using a dataloader similar to the original [Concept Bottleneck Molde (CBM)](https://github.com/yewsiang/ConceptBottleneck/tree/master/CUB) repository. They have a [preprocessed version of CUB](https://worksheets.codalab.org/worksheets/0x362911581fcd4e048ddfd84f47203fd2) images and associated concept attributes and species labels, which you can download; or you can look to the [Concept Embedding Model (CEM)](https://github.com/mateoespinosa/cem/tree/main/cem/data/CUB200/class_attr_data_10) repository; we specifically used these pkl files. You can override the [attribute labels](https://github.com/yewsiang/ConceptBottleneck/blob/master/CUB/dataset.py#L74) with our loaded in soft labels. Recall, at present `CUB-S` is a relabeling of a subset of the test set.

`cub_s_labels.json` is structured as follows: 
* Each key is the id of an image in the `CUB` test set. These integers match directly with the test set from ....
* Keys maps to lists of soft labels flattened for all concepts for that image. Each participant labeled all concepts for a single image. 
Some images have been labeled by multiple people; most were only labeled by one person. In the case of many labels, the outer lists represent the label extracted per annotator.

The flattened concepts are of length 312, corresponding to all original binary concepts. [Koh et al](https://github.com/yewsiang/ConceptBottleneck) filter these down to 112, but you could explore using all concepts (as humans do express some probability over many of them!) In this work though, we apply the same filtering, see [indices here](https://github.com/yewsiang/ConceptBottleneck/blob/master/CUB/generate_new_data.py#L71). We do encourage playing with other ways to use CUB-S as well!

If the json is too confusing, we recommend starting with the less processed `raw_cub-s_human_data.csv`, which has columns representing the following: 
* subject: unique id randomly generated for a given annotator.
* concept_group: the concept group name being annotated
* evalAttrsUncs: soft labels provided by the annotator for the particular concept; each annotation is between 0 and 100. If an attribute is not included, then the annotator did not select it from the checkbox option (we consider these as being 0; i.e., not present / possible / "off"). 
* img_id: integer corresponding to the id in the `json`, and the CUB test set [pkl](https://github.com/mateoespinosa/cem/tree/main/cem/data/CUB200/class_attr_data_10) file. 
* filename: original CUB image filename, as per the test set pkl file. 
* label: category assigned to the image according to the [CIFAR-10 test set](https://www.cs.toronto.edu/~kriz/cifar.html).
* rt: time spent (msec) on a given page, by an annotator.
* time_elapsed: total time (msec) an annotator has taken on the experiment so far; note: instruction reading time is included from the first annotation.

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

If you have any questions about `CUB-S` use, elicitation, and/or creation, please do not hesitate to add a GitHub Issue and/or reach out to Katie Collins (`kmc61@cam.ac.uk`) and Matthew Barker (`mrlb3@cam.ac.uk`).

