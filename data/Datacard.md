### DataCard for ISIC2024 SLICE-3D Dataset

---

### Dataset composition
- **Data types:** JPEG image crops; HDF5 image containers; CSV metadata files.  
- **Image field of view:** Crops correspond to **15×15 mm** lesion patches extracted from 3D TBP captures.  
- **Label types:** **Strong labels** (histopathology-confirmed malignant/benign) and **weak labels** (clinician-assessed non-biopsied benign).  
- **Scale:** **401,064 files** total; **2.69 GB** reported dataset size for training images.

---

### Files and sizes

| **File or folder** | **Description** | **Notes** |
|---|---:|---|
| **train-image/** | Training JPEG image crops | Part of 2.69 GB dataset |
| **train-image.hdf5** | Training images keyed by **isic_id** | Single HDF5 container |
| **test-image.hdf5** | Test images placeholder; replaced by hidden test set during evaluation | Hidden test set ≈ 500k images |
| **train-metadata.csv** | Training metadata including **target** and lesion/patient fields | Contains labels and lesion-level features |
| **test-metadata.csv** | Test subset metadata | For local validation only |
| **sample_submission.csv** | Example submission format | One probability per **isic_id** |

---

### Metadata summaries

#### Key identifiers and demographics
| **Field** | **Type** | **Notes** |
|---|---:|---|
| **isic_id** | string | Unique case identifier |
| **patient_id** | string | Unique patient identifier |
| **lesion_id** | string | Unique lesion identifier (train only) |
| **age_approx** | integer | Approximate patient age |
| **sex** | categorical | Patient sex |
| **anatom_site_general** | categorical | Anatomical location of lesion |

#### Diagnostic and label fields
| **Field** | **Type** | **Notes** |
|---|---:|---|
| **target** | binary {0,1} | **0 = benign**, **1 = malignant**; present in train only |
| **iddx_full, iddx_1 … iddx_5** | categorical | Multi-level diagnosis taxonomy |
| **mel_mitotic_index** | numeric | Melanoma mitotic index (pathology) |
| **mel_thick_mm** | numeric | Melanoma thickness in mm (pathology) |

#### Image and lesion-level quantitative features
| **Field** | **Type** | **Notes** |
|---|---:|---|
| **image_type** | categorical | Image capture modality |
| **tbp_tile_type** | categorical | TBP tile / lighting descriptor |
| **tbp_lv_color_*** | numeric | Color channel statistics per lesion |
| **tbp_lv_shape_*** | numeric | Shape descriptors (area, perimeter, symmetry) |
| **tbp_lv_confidence_*** | numeric | Confidence scores for lesion detection |
| **tbp_lv_centroid_x, tbp_lv_centroid_y** | numeric | Lesion coordinates within tile |
| **clin_size_long_diam_mm** | numeric | Clinical maximum diameter in mm |

---

### License and citation
- **License:** Creative Commons Attribution-NonCommercial 4.0 International (**CC BY-NC 4.0**).  
- **Citation:** *International Skin Imaging Collaboration. SLICE-3D 2024 Challenge Dataset. International Skin Imaging Collaboration `https://doi.org/10.34970/2024-slice-3d` [(doi.org in Bing)](https://www.bing.com/search?q="https%3A%2F%2Fdoi.org%2F10.34970%2F2024-slice-3d") (2024).*

---

### Usage notes and limitations
- **Submission format:** One probability per **isic_id**; follow **sample_submission.csv** format.  
- **Label confidence:** Treat clinician-assigned benign labels as **lower confidence** than histopathology-confirmed labels; consider weighting or separate handling during training.  
- **Hidden test set:** Official evaluation replaces `test-image.hdf5` with a **hidden test set** (≈500k images); design inference pipeline to stream or batch-read large HDF5 inputs.  
- **Preprocessing suggestions:** Preserve lesion scale (15×15 mm) when resizing; normalize color channels per TBP lighting descriptors; consider metadata-aware augmentation (age, site, lesion size).  
- **Ethics and redistribution:** Redistribution must comply with **CC BY-NC 4.0** and include the dataset citation. Avoid using the data for commercial purposes.  
- **Evaluation caveat:** Mixed label sources can bias metrics; report separate results on histopathology-confirmed subset when possible.

---

### Quick checklist for preparing a submission
- **1.** Confirm inference outputs one probability per **isic_id** matching `sample_submission.csv`.  
- **2.** Ensure pipeline can read HDF5 containers and handle large hidden test set.  
- **3.** Account for weak labels during training (sample weighting, label smoothing, or separate validation).  
- **4.** Include dataset citation and adhere to **CC BY-NC 4.0** in any redistribution.  

---

**End of DataCard**
