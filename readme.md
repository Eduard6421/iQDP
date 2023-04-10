# iQPP: Image Query Performance Prediction Benchmark (Official Repo, Accepted at SIGIR 2023)

We propose the first benchmark for image query performance prediction (iQPP). First, we establish a set of four data sets (PASCAL VOC 2012, Caltech-101, ROxford5k and RParis6k) and estimate the ground-truth difficulty of each query as the average precision or the precision@𝑘, using two state-of-the-art image retrieval models. Next, we propose and evaluate twelve pre-retrieval and post-retrieval query performance predictors. We release our code as an open source, under the MIT license. However, the retrieval methods and the datasets each have their own open source license.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](/LICENSE)

## Retrieval methods

1. [Radenovic et al. [TPAMI 2019]](https://github.com/filipradenovic/cnnimageretrieval-pytorch) - released under MIT License.
2. [Revaud et al. [ICCV 2019]](https://github.com/naver/deep-image-retrieval) - released under BSD-3 Clause license.

## Datasets

1. [PASCAL VOC 2012 (Download)](http://host.robots.ox.ac.uk/pascal/VOC/) - [Flickr terms of use](https://www.flickr.com/help/terms)
2. [Caltech-101 (Download)](https://data.caltech.edu/records/mzrjq-6wc02) - [Caltech Data terms of use](https://library.caltech.edu/search/caltechdata#terms)
3. [ROxford5k (Download)](http://cmp.felk.cvut.cz/revisitop/) - [Flickr terms of use](https://www.flickr.com/help/terms) and [Dataset Terms of Access](https://www.robots.ox.ac.uk/~vgg/terms/dataset-group-2-access.html)
4. [RParis6k (Download)](http://cmp.felk.cvut.cz/revisitop/) - [Flickr terms of use](https://www.flickr.com/help/terms) and [Dataset Terms of Access](https://www.robots.ox.ac.uk/~vgg/terms/dataset-group-2-access.html)

---

## 📝 Table of Contents <a name = "tabel_of_contents"></a>

- [iQPP: Image Query Performance Prediction Benchmark (Official Repo)](#iqpp-image-query-performance-prediction-benchmark-official-repo)
  - [Retrieval methods](#retrieval-methods)
  - [Datasets](#datasets)
  - [📝 Table of Contents ](#-table-of-contents-)
  - [About ](#about-)
  - [Getting Started ](#getting-started-)
    - [Installing Prerequisites ](#installing-prerequisites-)
  - [Usage ](#usage-)
    - [For Radenovic et al. \[TPAMI 2019\] ](#for-radenovic-et-al-tpami-2019-)
    - [For Revaud et al. \[ICCV 2019\] ](#for-revaud-et-al-iccv-2019-)
    - [Run the QPP models.](#run-the-qpp-models)
    - [Compute the correlations.](#compute-the-correlations)
  - [⛏️ Developed with ](#️-developed-with-)
  - [Citation ](#citation-)
  - [🎉 Acknowledgements ](#-acknowledgements-)

## About <a name = "about"></a>

This repository holds the source code four our paper "Image Query Performance Prediction". It holds the label generation scripts, modifications to the retrieval models and implementations of the studied models.
Due to the naming of the retrieval model git repositories, you  will often see the retrieval methods described as "cnnimageretrieval" and "deepretrieval". These are in fact the repository names of the work of Radenovic et al. , respectively Revaud et al.

The project is structured as following:

     IQPP/
          Datasets/ (contains an extensive description of the dataset format and a short tutorial in order to download the content and prepare it for CBIR)
          Embeddings/ (folder which will be populated with the embeddings generated by the retrieval methods)
          Folds/ ( contains the 5-folds used for training the supervised models in order for further researcher to obtain consistent replication)
          QPP_Methods/ (the codebase for the employed content-based image retrieval models)
          Results/ (folder which will contain csv files containing predicted scores after running the QPP methods)
          Retrieval_Methods/ (folder which contains the studied retrieval method implementation. these are forks of the original git repository of the authors)
          readme.md ( A readme file which contains the current tutorial)

## Getting Started <a name = "getting_started"></a>

### Installing Prerequisites <a name = "prerequisites"></a>

Prerequisites can be seen in the requirements.txt text file.
However I would like to point out that Revaud et al. retrieval model cannot be run on an newer scikit-learn version. As per the [issue](https://github.com/naver/deep-image-retrieval/issues/27) the authors recommend the version 0.20.2.



Run the following command in order to install the prerequisites:

```
pip install -r requirements.txt
```

## Usage <a name="usage"></a>

There are multiple steps involved in running the benchmark. In order to replicate our results you will have to follow the next steps:
1. Download the datasets from the provided links.
2. Copy all the images in the corresponding dataset folder under the "jpg" subfolder. For example, in the case of ROxford5K you must copy the images in "Datasets/ROxford5k/jpg".
3. Run the retrieval methods. The methods do not have a unified interface, and as such, require method specific arguments and changes. All of these are  described in detail below. 

  ### For Radenovic et al. [TPAMI 2019] <a name="radenovic"></a>
  
  3.1.2. Decide on which dataset you want to run the retrieval model. Available options are:
      
```
roxford5k
roxford5k_drift_cnn
rparis6k
rparis6k_drift_cnn
pascalvoc_700
pascalvoc_700_train
pascalvoc_700_drift_cnn
caltech101_700
caltech101_700_train
caltech101_700_drift_cnn
```
      
  The methods marked as drift denote the queries were identified during the adapted query drift technique.

  The embeddings of the query and dataset images will be saved in "Embeddings/CNN_Image_Retrieval/".

  The top results will be saved in the folder "Results/".
        
  3.1.3. Select the metric you want to show (p@100 or ap) by changing which value you want to be printed. In "Retrieval_Methods\Radenovic-et-al\radenovic-et-al\cirtorch\utils\evaluate.py" comment one of the lines 135/137. By default, AP is selected.
            
  3.1.4. Run the model with the following instructions:  

```
python -m cirtorch.examples.test --gpu-id "0" --network-path "retrievalSfM120k-resnet101-gem" --datasets "caltech101_700" \ --whitening "retrieval-SfM-120k" --multiscale "[1, 1/2**(1/2), 1/2]"
```
  
  ### For Revaud et al. [ICCV 2019] <a name="revaud"></a>

  3.2.1. Decide on which dataset you want to run the retrieval model. Available options are:
  
```
ROxford5K
ROxford5K_Drift
RParis6K
RParis6K_Drift
PascalVOC_700        
PascalVOC_700_Train
PascalVOC_700_Drift
Caltech101_700
Caltech101_700_Train
Caltech101_700_Drift
```
 
  3.2.2. Select which metric you want to use. In "Retrieval_Methods\Revaud-et-al\deep-image-retrieval\dirtorch\test_dir.py", call either 
    ```eval_query_AP``` or ```eval_query_top_k``` by commenting the specific line (182 or 183, depending on your choice).

  3.2.3. Run the model with the following command:
  
```
python -m dirtorch.test_dir --dataset 'Caltech101_700' --checkpoint '/notebooks/deep-image-retrieval/Resnet101-AP-GeM.pt'
```
### Run the QPP models. 

You must follow the next steps in order to run the query performance prediction methods:\
4.1. cd QPP_Methods/Score_Variance

4.2. Run the model with the following command:
```
python3 main.py --dataset roxford5k --method cnnimageretrieval --metric ap
```

Available options for methods are:

```
cnnimageretrieval
deepretrieval
```
Available options for metrics are:
```
ap
p@100
```

### Compute the correlations.




## ⛏️ Developed with <a name = "developed_with"></a>
- [Pytorch](https://pytorch.org/) - Deep Learning Library.

## Citation <a name="citation"></a>

```
@article{poesina2023iqpp,
   title={iQPP: A Benchmark for Image Query Performance Prediction},
   author={Poesina, Eduard and Ionescu, Radu Tudor and Mothe, Josiane},
   journal={arXiv preprint arXiv:2302.10126},
   year={2023}
}
```

## 🎉 Acknowledgements <a name = "acknowledgement"></a>

We thank all researchers and data annotators behind the datasets and the content-based image retrieval methods!
