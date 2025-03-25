# genomics-exercise answers


## 1. How many mutations are in the cancer genome?
**Answer**: 4631 mutations are in the cancer genome  
```python
# count the number of rows in the WGS_mutations.bed file directly 
num_mutations = sum(1 for _ in open("WGS_mutations.bed"))
```

## 2. What is the median size (in base pairs) of regions in the replication timing file?
**Answer**: median replication region size is 1000.0 bp  
```python
# based on file Replication_timing.bed, end - start = region sizes
region_sizes = [(int(line.split()[2]) - int(line.split()[1])) for line in open("Replication_timing.bed")]
median_size = np.median(region_sizes)
```

## 3. List the top 5 earliest replicating region in the genome.
**Answer**: 
```txt
('chr10', 80941499, 80942499, 73.1628)
('chr10', 80942499, 80943499, 73.1627)
('chr10', 80940499, 80941499, 73.1621)
('chr10', 80943499, 80944499, 73.1618)
('chr10', 80939499, 80940499, 73.1607)
```
```python
# sort Replication_timing.bed file by column 4 (replication time) in descending order, take the first 5 rows  
sorted_regions = sorted(regions, key=lambda x: -x[3])[:5]
```

## 4. How many mutations are there in the 10,000 latest replicating regions?
**Answer**: there're 219 mutations in the 10,000 latest replicating regions
```python
# read the file Replication_timing.bed then sort it
late_regions = sorted(regions, key=lambda x: x[3])[:10000]

# turn it into bed format
late_bed = pybedtools.BedTool([f"{chrom}\t{start}\t{end}" for chrom, start, end, _ in late_regions])

# read the file WGS_mutations.bed then find the overlaps
overlaps = mutation_bed.intersect(late_bed, u=True)
mutation_count = len(overlaps)
```

## 5. Is the mutation rate generally higher in early or late replicating regions?
**Answer**: the mutation rate is generally higher in early replication regions (0.005%)
```python
# read the file Replication_timing.bed then sort it 
early_regions = sorted(regions, key=lambda x: -x[3])[:10000] 
late_regions = sorted(regions, key=lambda x: x[3])[:10000]

# turn it into bed format
early_bed = pybedtools.BedTool([f"{chrom}\t{start}\t{end}" for chrom, start, end, _ in early_regions])
late_bed = pybedtools.BedTool([f"{chrom}\t{start}\t{end}" for chrom, start, end, _ in late_regions])

# read the file WGS_mutations.bed then find the overlaps
early_mutations = mutation_bed.intersect(early_bed, u=True)
late_mutations = mutation_bed.intersect(late_bed, u=True)

# calculation
early_bp = sum(end - start for _, start, end, _ in early_regions)
late_bp = sum(end - start for _, start, end, _ in late_regions)
early_rate = len(early_mutations) / early_bp
late_rate = len(late_mutations) / late_bp
```

## 6. What type of DNA repair defect is this cancer likely to have based on the difference in mutation rate of the early and late replicating regions? (You will probably need to do literature search)
**Answer**: it might be DNA mismatch repair (MMR) <a target="_blank" href="https://doi.org/10.1038/nature14173">(1)</a>

## 7. What cancer type do you think this sample is from?
**Answer**: it might be colon cancer <a target="_blank" href="https://doi.org/10.1016/j.cell.2017.01.002">(2)</a> <a target="_blank" href="https://www.cancer.gov/publications/dictionaries/cancer-terms/def/mismatch-repair-deficiency">(3)</a>

## 8. Using GENCODE_known_canonical_TSS.bed, write a script to generate a file containing all genes with bidirectional promoters (i.e. non-overlapping promoters that are at most 1kb apart on opposing strands - see schematic below).
**Answer**: <a target="_blank" href="https://github.com/HXiangru/hku/blob/main/bidirectional_promoters.bed.txt">bidirectional_promoters.bed</a>
```python
# read the file, extract information about adjacent genes
for i in range(len(df) - 1):
    chr1, start1, end1, gene1, strand1 = df.iloc[i][["chr", "start", "end", "gene", "strand"]]
    chr2, start2, end2, gene2, strand2 = df.iloc[i + 1][["chr", "start", "end", "gene", "strand"]]

# determine whether it is a bidirectional promoter
    if chr1 == chr2 and strand1 != strand2 and abs(start1 - start2) <= 1000:
        bidirectional_genes.append([chr1, start1, end1, gene1, strand1, start2, end2, gene2, strand2])
```
