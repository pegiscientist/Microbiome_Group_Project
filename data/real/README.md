# Real Microbiome Data — Completed Phase 4 Analysis

This directory contains the **completed real-data component (Phase 4)** of the BIOT670I *Microbiome Abundance Consensus Machine* project. It documents preprocessing, quality-control results, four differential-abundance (DA) analyses, cross-method agreement, consensus identification, and scientific interpretation for a publicly available colorectal cancer (CRC) microbiome dataset.

**Phase 4 status: Complete.** See the [main project README](../../README.md) for the full simulation and real-data study.

## Dataset and provenance

- **Dataset identifier:** `crc_baxter`
- **Source:** MicrobiomeHD; Baxter et al. (2016)
- **Study comparison:** CRC cases versus healthy controls (the adenoma group was excluded)
- **Source archive:** `crc_baxter/crc_baxter_results.tar.gz`
- **Dataset record:** https://zenodo.org/records/840333
- **Study DOI:** https://doi.org/10.1186/s13073-016-0290-3

The published dataset is used to examine *agreement among methods* under real biological conditions. Unlike the simulated-data experiments, the true differential-abundance status of each OTU is unknown.

## Data preparation and quality control

The original dataset included **122,510 OTUs**. The CRC-versus-healthy comparison contains **292 samples**, comprising **120 CRC** cases and **172 healthy** controls.

Recorded preprocessing settings and results:

| Item | Recorded value |
| --- | --- |
| Original OTUs | 122,510 |
| Retained OTUs | 1,133 |
| Removed OTUs | 121,377 |
| Minimum prevalence | 10% of samples (minimum 30 samples) |
| Minimum total reads per OTU | 20 |
| Zero fraction after filtering | 76.5% |
| Stored count-table orientation | Taxa × samples |
| Normalization applied during this preprocessing step | No |
| Aggregation applied during this preprocessing step | No |

The separate raw-count validation record reports **no invalid cells, negative counts, non-integer counts, or duplicate OTU IDs**, and its `validation_passed` flag is `true`. The provenance file `dataset_info.json` separately retains `counts_verified: false` and `metadata_verified: false`; those metadata flags have not been updated to reflect the separate count-validation result, and this README does not interpret them as proof of completed metadata validation.

**Preparation and validation files**
- [Dataset description](crc_baxter/dataset_info.json)
- [Count validation record](crc_baxter/crc_baxter_count_validation.json)
- [Preprocessing summary](crc_baxter/crc_baxter_preprocessing_summary.json)
- [Filtered counts](crc_baxter/crc_baxter_filtered_counts.csv)
- [Filtered metadata](crc_baxter/crc_baxter_filtered_metadata.csv)
- [CRC-versus-healthy sample metadata](crc_baxter/crc_baxter_crc_vs_healthy_metadata.csv)

## Differential-abundance analysis

Four methods were applied to the filtered CRC-versus-healthy dataset:

1. **CLR/Wilcoxon**
2. **ALDEx2 Welch**
3. **ANCOM-BC2**
4. **DESeq2**

The reported discovery criterion is **Benjamini–Hochberg (BH)-adjusted p-value < 0.05**.

### Significant OTUs by method

| Method | OTUs returned | Valid adjusted p-values | Significant OTUs |
| --- | ---: | ---: | ---: |
| CLR/Wilcoxon | 1,133 | 1,133 | 24 |
| ALDEx2 Welch | 1,133 | 1,133 | 0 |
| ANCOM-BC2 | 1,133 | 1,133 | 486 |
| DESeq2 | 1,133 | 584 | 20 |

The methods produced substantially different significance calls, particularly between ANCOM-BC2 and the other approaches. Note that DESeq2 returned valid adjusted p-values for **584 of the 1,133** OTUs; an OTU without a usable adjusted p-value is not interchangeable with a tested, nonsignificant OTU.

**Method-specific results**
- [CLR/Wilcoxon](crc_baxter/DA_results/CLR_Wilcoxon_results.csv)
- [ALDEx2 Welch](crc_baxter/DA_results/ALDEx2_Welch_results.csv)
- [ANCOM-BC2](crc_baxter/DA_results/ANCOM_BC2_results.csv)
- [DESeq2](crc_baxter/DA_results/DESeq2_results.csv)
- [Method summary](crc_baxter/DA_results/crc_baxter_method_summary.csv)
- [Combined method results](crc_baxter/DA_results/crc_baxter_all_method_results.csv)

## Cross-method agreement

The completed analysis assessed overlap of significant OTU sets, including **pairwise Jaccard similarity**. Jaccard similarity measures the size of the intersection divided by the size of the union of two sets of discoveries.

Selected off-diagonal values from the saved Jaccard matrix:

| Pair of methods | Jaccard similarity |
| --- | ---: |
| CLR/Wilcoxon vs. ANCOM-BC2 | 0.0324 |
| CLR/Wilcoxon vs. DESeq2 | 0.0476 |
| ANCOM-BC2 vs. DESeq2 | 0.0412 |
| ALDEx2 Welch vs. each other method | 0 |

ALDEx2 Welch had no significant OTUs under the recorded criterion. Thus, the Jaccard value comparing ALDEx2 with itself is recorded as undefined (`NA`) rather than interpreted as perfect agreement.

![Jaccard similarity heatmap](crc_baxter/DA_results/crc_baxter_jaccard_heatmap.png)

- [Jaccard similarity matrix](crc_baxter/DA_results/crc_baxter_jaccard_matrix.csv)
- [Method agreement summary](crc_baxter/DA_results/crc_baxter_method_agreement_summary.csv)

## Consensus analysis

The consensus analysis counts how many of the four methods support each OTU as significant.

| Minimum number of supporting methods | OTUs |
| --- | ---: |
| At least 1 of 4 | 494 |
| At least 2 of 4 | 34 |
| At least 3 of 4 | 2 |
| All 4 | 0 |

The completed **3-of-4 consensus identified two OTUs**, both supported by CLR/Wilcoxon, ANCOM-BC2, and DESeq2:

- `...;f__Lachnospiraceae;g__Anaerostipes;s__;d__denovo879`
- `...;f__Lachnospiraceae;g__;s__;d__denovo406`

These are taxonomic strings as represented in the exported results, not independently validated species-level identifications.

**Consensus outputs**
- [All-taxa consensus table](crc_baxter/DA_results/crc_baxter_consensus_all_taxa.csv)
- [3-of-4 consensus OTUs](crc_baxter/DA_results/crc_baxter_consensus_3of4.csv)
- [Final consensus details](crc_baxter/DA_results/crc_baxter_final_consensus_details.csv)

## Scientific interpretation and limitations

The completed analysis shows **substantial disagreement among DA methods applied to the same real microbiome dataset**. Only two OTUs met the 3-of-4 consensus threshold, and no OTU was significant in all four methods.

**Interpretation requires these distinctions:**

- **Unknown ground truth:** The real dataset does not provide known true DA labels. Empirical FDR and sensitivity cannot be calculated directly, and agreement alone does not prove biological correctness.
- **Effect direction:** The completed comparison does not establish agreement in effect direction for the identified OTUs.
- **Clinical covariates:** The analysis does not adjust for clinical covariates.
- **ANCOM-BC2:** The reported 486 discoveries are based on BH-adjusted p-values; they are **not confirmed pseudo-count-robust calls**.
- **Different valid test counts:** DESeq2 provided fewer valid adjusted p-values than the other methods, which matters when comparing discovery totals and agreement.

These are **limitations of the completed analysis**, not uncompleted analytical phases.

Read the [final Phase 4 interpretation](crc_baxter/DA_results/crc_baxter_final_interpretation.txt) for the source results and summary.

## Directory guide

```text
data/real/
├── README.md
└── crc_baxter/
    ├── dataset_info.json
    ├── crc_baxter_results.tar.gz
    ├── crc_baxter_count_validation.json
    ├── crc_baxter_preprocessing_summary.json
    ├── crc_baxter_filtered_counts.csv
    ├── crc_baxter_filtered_metadata.csv
    ├── crc_baxter_crc_vs_healthy_metadata.csv
    └── DA_results/
        ├── CLR_Wilcoxon_results.csv
        ├── ALDEx2_Welch_results.csv
        ├── ANCOM_BC2_results.csv
        ├── DESeq2_results.csv
        ├── crc_baxter_all_method_results.csv
        ├── crc_baxter_method_summary.csv
        ├── crc_baxter_jaccard_matrix.csv
        ├── crc_baxter_jaccard_heatmap.png
        ├── crc_baxter_consensus_all_taxa.csv
        ├── crc_baxter_consensus_3of4.csv
        ├── crc_baxter_final_consensus_details.csv
        └── crc_baxter_final_interpretation.txt
```

This is a selected file listing; additional exported summaries are available in `DA_results/`.

## Reproducibility and data stewardship

The [project analysis code](../../notebooks/biot670i_microbiome_project.py), source archive, preprocessing records, filtered tables, and method-specific results document the saved real-data workflow. When rerunning the analysis, use the recorded sample groups and filtering thresholds and check method-specific dependency versions and settings.

The data source is publicly documented, but downstream users should verify the source dataset's terms of use before redistributing or reusing files. Sensitive, restricted, or personally identifiable data should not be committed.

## Conclusion

The completed Phase 4 analysis demonstrates that the four DA methods yield markedly different sets of significant OTUs in the Baxter CRC dataset. Cross-method agreement is limited, with **two OTUs meeting the 3-of-4 rule**. This supports the project's broader conclusion that microbiome DA results should be interpreted in light of method differences and agreement, while recognizing that consensus alone cannot establish ground-truth correctness.

For the known-truth simulation benchmark and consensus-rule performance, see the [simulated-data README](../simulated/README.md).

## Reference

Baxter et al. (2016). *Genome Medicine*. https://doi.org/10.1186/s13073-016-0290-3
