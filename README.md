# datasets_ml_all_ads_1M
## A MAGAZINEgts Ground-Truth Dataset for Softalk magazine (1980-84)
This is the 'seed' dataset of source page and label mask images of all ads in Softalk magazine based on the MAGAZINEgts ground-truth storage format. The Max_pixels setting is 1M pixels, and all page and label mask images are in PNG format.

There are currently 7,109 elements in what will be a eventually be a dataset of 7,157 ads appearing in Softalk magazine (1980-84). The dataset will be complete once a small number of data inconsistencies are resolved. 

> Update: Please see section below about the addition of 'Non-case Computational Complement Subsets for Document Structure Model Training'

Please see our [#DATeCH2017](https://www.researchgate.net/publication/317240599_The_MAGAZINE_GTS_format_an_integrated_document_structure_and_content_depiction_model_supporting_eResearch_and_machine-learning_at_the_Internet_Archive) and [#DATeCH2019](https://www.researchgate.net/publication/332625805_MAGAZINEgts_and_dhSegment_Using_a_Metamodel_Subgraph_to_Generate_Synthetic_Data_of_Under-Sampled_Complex_Document_Structures_for_Machine-Learning) posters for additional information about the #MAGAZINEgts ground-truth format providing integrated complex document structure and content depiction models based on an ontological "stack" of #cidocCRM, FRBRoo, and PRESSoo standards. 

The [XML-based MAGAZINEgts file (~13+ MB) for Softalk magazine is here](https://archive.org/download/softalkapple/softalkapple_publication.xml) as part of the [Softalk magazine collection at the Internet Archive](https://archive.org/details/softalkapple?sort=date). The growing set of page images and machine-learning label mask images for use with the MAGAZINEgts metamodel and metadata are available [here on GitHub](https://github.com/SoftalkAppleProject/datasets_ml_all_ads_1M).

![Small view of DATeCH2019 poster](/readme_etc/salmons_babitsky_datech2019_poster_small.png)

This dataset is to be used for training machine learning models to recognize magazine advertisements. This dataset includes both actual and predicted bounding-box dimensions for each ad in the magazine. The predicted bounding-box is based on the PRESSoo Issuing Rules of the Softalk Advertising Model contained in the Metamodel partition of the MAGAZINEgts file.

Note that this dataset is the simplest 'seed' dataset which will initially be of limited value for model training. We need, for example, to generate a complementary set of Softalk pages with no ads on the page to be incorporated into the training, validation, and test training data subsets. This 'seed' dataset, however, can be used to generate any number of alternative and more detailed training datasets based on mapping these ads to various parameter-patterns of the PRESSoo Issuing Rules for the Advertising Model of Softalk magazine found in the Metamodel partition of the MAGAZINEgts file describing the Softalk magazine collection at the Internet Archive. 

For example, this 'seed' dataset can be used to generate an #ML model-training dataset that lets the model understand the interrelationships between an advertisement's size and shape in terms of the allowable positions on a page for that ad. Using these page images and the 1M max_pixel all_ads dataset elements in the Metadata partition of the Softalk magazine MAGAZINEgts file, a new set of appropriately-colored label masks can be generated using the ad_spec bounding-box location provided by the ground-truth 'actual' measures provided by this 'seed' dataset. 

We intend to integrate Stanford DAWN's [Snorkel framework](https://www.snorkel.org/) to the *FactMiners Toolkit* to handle the _labeling_, _transformation_, and _slicing_ functions that such model-training dataset generation requires. In the meantime, here is a screenshot of an Excel spreadsheet PivotTable of the distribution of advertisements in the 48 issues of Softalk magazine:

![Screenshot of Excel PivotTable showing distribution of ads in Softalk](/readme_etc/datech2019_admodel_excel_pivot_table.png)

> NOTE: This PivotTable shows an early total of 7,164 total ads in the Ground-Truth branch of the #MAGAZINEgts DocumentStructure partition. This early total included 'dirty data' entries, including specs for known ads that appear on missing pages from the current state of the scan repository. These "to be resolved" entries are not included in this #MachineLearning model-training dataset. This early PivotTable-based examination of the distribution of Softalk ads, however, is still very reflective of the distribution of items in this #ML model training dataset.   

## Non-case Computational Complement Subsets for Document Structure Model Training

This 'seed' dataset of all advertisements in Softalk magazine now includes a dataset of 2,288 non-case examples of all Softalk pages WITHOUT an advertisement on them. These page and their 'blank' associated label images are found in a sibling subdirectory, **noncase**, containing its **images** and **label** subdirectories. Model trainers may pull matched pairs of these non-case entries into their training, evaluation, and test subsets to create balanced distributions of case and non-case training elements.

While considering the need to generate the non-case subsets needed for #MachineLearning model training, I was forced to consider the boundrary between _explicit_ and _computational_ metadata as part of the #MAGAZINEgts ground-truth storage format. As you will see when exploring the _softalk_publication.xml_ file, there is no explicit branch for the non-case subset in the **all_ads** ML dataset in the **Metadata** partition. This lack of explicit stored-representation of the non-case subset reflects the set-theoretic nature of this and similar subsets. That is, the simplest description of the non-case elements is that they are "everything but" the document structure of interest in the "main" model training dataset. So, in this case of identifying the subset membership for pages that are not cited in the Advertisers Index of Softalk, we simply need to compute the _complement of the AdIndex_ which is the complete set of Softalk's page with advertising. For each of these complementary instances, the bounding-box dimensions of the non-existent, in this case advertisement, document structure on that page is a (0, 0, 0, 0) rectangle. Both these membership-defining elements for the non-case subset are computable based on the explicit data in the Document Structure and Metamodel partitions of the #MAGAZINEgts format.

For example, the full non-case dataset for this 'seed' **all_ads** model-training dataset can be computationally generated by reference to the **Metadata/Leaf2ppg_map** and the **DocumentStructure/Advertisement/AdIndex** branches of the #MAGAZINEgts file for Softalk magazine. The generation of the *all_ads* non-case dataset for Softalk advertisements was added to the **FactMiners Toolkit** through the addition of a single Xquery query onto the _softalk_publication.xml_ file -- acutally done via a direct query onto the Toolkit's BaseX XML database -- together with the addition of a 22-line method, **generate_structure_noncase_dataset**, to the tool's Python implementation.

*Note:* As per the PRESSoo Issuing Rules for the Advertising Model of Softalk magazine described in the **Metamodel** partition of the #MAGAZINEgts file, Softalk's _Ad Index_ is found in the **DocumentStructure** branch of this Reference Model instance of this ground-truth storage format. Each issue of Softalk had a printed _Advertiser Index_ in addition to the _Table of Contents_, etc.. Most reasearcher- or tool-generated indices for other document structures of the magazine are of the derived, or non-explicit, type and therefore their data will be found in the _Metadata_ partition of a #MAGAZINEgts file.

More as it evolves...
> -- Jim Salmons --
FactMiners and The Softalk Apple Project
Broomfield, Colorado USA
