# Unix_assignment    
## Data Inspection
### Attributes of fang_et_al_genotypes.txt
```
$ ls -lh fang_et_al_genotypes.txt
$ du -h fang_et_al_genotypes.txt
$ wc -l fang_et_al_genotypes.txt
$ awk '{print NF; exit}' fang_et_al_genotypes.txt
```
By inspecting this file I learned that:
1. File Size: 11 MB (ls -lh result), 6.7 MB (du -h result)
2. Number of Lines: 2783
3. Number of Columns: 986
### Attributes of snp_position.txt
```
$ ls -lh snp_position.txt
$ du -h snp_position.txt
$ wc -l snp_position.txt
$ awk '{print NF; exit}' snp_position.txt
```
By inspecting this file I learned that:
1. File Size: 81 KB (ls -lh result), 49 K (du -h result)
2. Number of Lines: 984
3. Number of Columns: 15

## Data Processing
```
$ awk -f transpose.awk fang_et_al_genotypes.txt > transposed_fang.txt
$ join -1 1 -2 1 -a 1 -e "NA" transposed_fang.txt snp_position.txt > joined_transposed_fang_snp.txt
```
### Maize Data
```
$ awk 'NR == 3 || NR == 4 || ($3 ~ /^(ZMMIL|ZMMLR|ZMMMR)$/) {print}' transposed_join.txt > maize_group.txt
$ awk -f transpose.awk maize_group.txt > t_maize_group.txt
```
Extract genotype, chromosome, position in maize group (Group = ZMMIL, ZMMLR, and ZMMMR).
```
$ awk 'NR == 2 {for(i=1;i<=NF;i++) if($i == "unknown") a[++c]=i} {for(i=1;i<=c;i++) printf "%s%s", $a[i], (i==c ? "\n" : "\t")}' maize_group.txt | column -t > maize_unknown_positions.txt
$ awk 'NR == 2 {for(i=1;i<=NF;i++) if($i == "multiple") a[++c]=i} {for(i=1;i<=c;i++) printf "%s%s", $a[i], (i==c ? "\n" : "\t")}' maize_group.txt | column -t > maize_multiple_positions.txt
```
Extract SNPs with unknown AND multiple positions
```
$ for chr in {1..10}; do
    awk -v chr="$chr" '$1 == chr {print}' t_maize_group.txt | sort -k2,2n | awk '{gsub(/NA/, "?"); print}' > maize_chr${chr}_ascending.txt
done
$ for chr in {1..10}; do
    awk -v chr="$chr" '$1 == chr {print}' t_maize_group.txt | sort -k2,2nr | awk '{gsub(/NA/, "-"); print}' > maize_chr${chr}_descending.txt
done
```
A loop for sorting maize group files by chromosome number and reorder by position.
```
mkdir maizegroup
$ mv maize_chr* maizegroup/
$ mv maize_multiple_positions.txt maize_unknown_positions.txt maizegroup/
```
Move maize files to new folder.


### Teosinte Data
```
$ awk 'NR == 3 || NR == 4 || ($3 ~ /^(ZMPBA|ZMPIL|ZMPJA)$/) {print}' transposed_join.txt > teosinte_group.txt
$ awk -f transpose.awk teosinte_group.txt > t_teosinte_group.txt
```
Extract genotype, chromosome, position in teosinte group (Group = ZMPBA, ZMPIL, and ZMPJA).
```
$ awk 'NR == 2 {for(i=1;i<=NF;i++) if($i == "unknown") a[++c]=i} {for(i=1;i<=c;i++) printf "%s%s", $a[i], (i==c ? "\n" : "\t")}' teosinte_group.txt | column -t > teosinte_unknown_positions.txt
$ awk 'NR == 2 {for(i=1;i<=NF;i++) if($i == "multiple") a[++c]=i} {for(i=1;i<=c;i++) printf "%s%s", $a[i], (i==c ? "\n" : "\t")}' teosinte_group.txt | column -t > teosinte_multiple_positions.txt
```
Extract SNPs with unknown AND multiple positions
```
$ for chr in {1..10}; do
    awk -v chr="$chr" '$1 == chr {print}' t_teosinte_group.txt | sort -k2,2n | awk '{gsub(/NA/, "?"); print}' > teosinte_chr${chr}_ascending.txt
done
$ for chr in {1..10}; do
    awk -v chr="$chr" '$1 == chr {print}' t_teosinte_group.txt | sort -k2,2nr | awk '{gsub(/NA/, "-"); print}' > teosinte_chr${chr}_descending.txt
done
```
A loop for sorting maize group files by chromosome number and reorder by position.
```
$ mkdir teosintegroup
$ mv teosinte_chr* teosintegroup/
$ mv teosinte_multiple_positions.txt teosinte_unknown_positions.txt teosintegroup/
```
Move maize files to new folder.
```
$ mkdir Unix_assignment_Yuwei
$ mv maizegroup teosintegroup Unix_assignment_Yuwei/
```
Move maize and teosinte folder.
## Upload to Github
```
$ git remote set-url origin git@github.com:YuweiZhang98/Unix_assignment.git
$ git add .
$ git commit -m "Initial commit"
$ git remote add origin git@github.com:YuweiZhang98/Unix_assignment.git
$ git push -u origin master
```

