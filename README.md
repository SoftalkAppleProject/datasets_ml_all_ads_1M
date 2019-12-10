# datasets_ml_all_ads_1M
## A MAGAZINEgts Ground-Truth Dataset for Softalk magazine (1980-84)
This is the 'seed' dataset of source page and label mask images of all ads in Softalk magazine based on the MAGAZINEgts ground-truth storage format. The Max_pixels setting is 1M pixels, and all page and label mask images are in PNG format.

There are currently 7,109 elements in what will be a eventually be a dataset of 7,157 ads appearing in Softalk magazine (1980-84). The dataset will be complete once a small number of data inconsistencies are resolved. 

> Update: Please see section below about the addition of 'Non-case Computational Complement Subsets for Document Structure Model Training'

Please see our [#DATeCH2017](https://www.researchgate.net/publication/317240599_The_MAGAZINE_GTS_format_an_integrated_document_structure_and_content_depiction_model_supporting_eResearch_and_machine-learning_at_the_Internet_Archive) and [#DATeCH2019](https://www.researchgate.net/publication/332625805_MAGAZINEgts_and_dhSegment_Using_a_Metamodel_Subgraph_to_Generate_Synthetic_Data_of_Under-Sampled_Complex_Document_Structures_for_Machine-Learning) posters for additional information about the #MAGAZINEgts ground-truth format providing integrated complex document structure and content depiction models based on an ontological "stack" of #cidocCRM, FRBRoo, and PRESSoo standards. 

The [XML-based MAGAZINEgts file (~13+ MB) for Softalk magazine is here](https://archive.org/download/softalkapple/softalkapple_publication.xml) as part of the [Softalk magazine collection at the Internet Archive](https://archive.org/details/softalkapple?sort=date). The growing set of page images and machine-learning label mask images for use with the MAGAZINEgts metamodel and metadata are available [here on GitHub](https://github.com/SoftalkAppleProject/datasets_ml_all_ads_1M).

This dataset is to be used for training machine learning models to recognize magazine advertisements. This dataset includes both actual and predicted bounding-box dimensions for each ad in the magazine. The predicted bounding-box is based on the PRESSoo Issuing Rules of the Softalk Advertising Model contained in the Metamodel partition of the MAGAZINEgts file.

Note that this dataset is the simplest 'seed' dataset which will initially be of limited value for model training. We need, for example, to generate a complementary set of Softalk pages with no ads on the page to be incorporated into the training, validation, and test training data subsets. This 'seed' dataset, however, can be used to generate any number of alternative and more detailed training datasets based on mapping these ads to various parameter-patterns of the PRESSoo Issuing Rules for the Advertising Model of Softalk magazine found in the Metamodel partition of the MAGAZINEgts file describing the Softalk magazine collection at the Internet Archive. 

For example, this 'seed' dataset can be used to generate an #ML model-training dataset that lets the model understand the interrelationships between an advertisement's size and shape in terms of the allowable positions on a page for that ad. Using these page images and the 1M max_pixel all_ads dataset elements in the Metadata partition of the Softalk magazine MAGAZINEgts file, a new set of appropriately-colored label masks can be generated using the ad_spec bounding-box location provided by the ground-truth 'actual' measures provided by this 'seed' dataset. 

We intend to integrate Stanford DAWN's [Snorkel framework](https://www.snorkel.org/) to the *FactMiners Toolkit* to handle the _labeling_, _transformation_, and _slicing_ functions that such model-training dataset generation requires. 

## Non-case Computational Complement Subsets for Document Structure Model Training

This 'seed' dataset of all advertisements in Softalk magazine now includes the dataset of 2,288 non-case examples of all pages WITHOUT an advertisement on them. These page and their (blank) associated label images are found in a sibling subdirectory, **noncase**, which contains the respective **images** and **label** directories. Model trainers may pull matched pairs of these non-case entries into their training, evaluation, and test subsets to create balanced distributions of case and non-case training elements.

While considering the need to generate the non-case subsets needed for #MachineLearning model training, I was forced to consider the boundrary between _explicit_ and _computational_ metadata as part of the #MAGAZINEgts ground-truth storage format. As you will see when exploring the _softalk_publication.xml_ file, there is no explicit branch for the non-case subset in the **all_ads** ML dataset. This lack of explicit representation reflects the set-theoretic nature of this data subset. That is, the simplest description of the non-case elements is that they are "everything but" the document structure of interest in the "main" model training dataset. So its subset membership is the _complement of the AdIndex_ where the bounding-box dimensions of the (non-existent) document structure on that page is a (0, 0, 0, 0) rectangle.

In fact, the full non-case dataset for the **all_ads** model-training dataset can be computationally generated for this document structure by reference to the _Metadata/Leaf2ppg_map_ and the _DocumentStructure/Advertisement/AdIndex_ branches of the #MAGAZINEgts file for Softalk magazine. In fact, the generation of the *all_ads* non-case dataset for advertisements was added to the FactMiners Toolkit through the addition of a single Xquery query onto the _softalk_publication.xml_ file -- acutally done via a direct query onto the Toolkit's BaseX XML database -- together with the addition of a 22-line method, **generate_structure_noncase_dataset** to the tool's Python implementation.

*Note:* As per the PRESSoo Issuing Rules for the Advertising Model of Softalk reflected in the Metamodel partition of the #MAGAZINEgts file, Softalk's Ad Index is found in the Document Structure branch of this instance as each issue of Softalk had a print Advertiser Index in addition to the Table of Contents. Most reasearcher-generated indices for other document structures are non-explicit in the print publication and, therefore, their data will be found in the _Metadata_ partition of a #MAGAZINEgts file.

More as it evolves...
> -- Jim Salmons --
FactMiners and The Softalk Apple Project
Broomfield, Colorado USA
