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
**Answer**: 
```python
# 

```

## 5. Is the mutation rate generally higher in early or late replicating regions?
**Answer**: 
```python
# 

```

## 6. What type of DNA repair defect is this cancer likely to have based on the difference in mutation rate of the early and late replicating regions? (You will probably need to do literature search)
**Answer**: 
```python
# 

```

## 7. What cancer type do you think this sample is from?
**Answer**: 
```python
# 

```

## 8. Using GENCODE_known_canonical_TSS.bed, write a script to generate a file containing all genes with bidirectional promoters (i.e. non-overlapping promoters that are at most 1kb apart on opposing strands - see schematic below).
**Answer**: 
```python
# 

```
