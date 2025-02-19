#UNIX Assignment

##Note on File Locatiosn

Source files and intermediate files located in /UNIX_Assignment folder, result files output to /Result_Files folder for organization

##Data Inspection

###Attributes of `fang_et_al_genotypes`

```
here is my snippet of code used for data inspection
$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
$ file fang_et_al_genotypes.txt
$ tail -n +2 fang_et_al_genotypes.txt | cut -f 3 | uniq -c
```

By inspecting this file I learned that:

1. 986 columns are present
2. The file is encoded as ASCII text
3. This creates a list of each unique group present in the file, and counts how many entries exist in each group - we're interested in ZMM** and ZMP** genotype data


###Attributes of `snp_position.txt`

```
here is my snippet of code used for data inspection
$ awk -F "\t" '{print NF; exit}' snp_position.txt
$ file snp_position.txt
$ du -h snp_position.txt
$ wc -l snp_position.txt
$ head -n 1 snp_position.txt

```

By inspecting this file I learned that:

1. 15 columns are present (tab separated)
2. The file is encoded as ASCII text
3. The file is 84K in size
4. The file has 984 lines (meaning 984 SNPs listed)
5. Column titles are listed - column 1 is SNP_ID, column 3 is Chromosome, column 4 is Position 


##Data Processing

###Maize Data

```
here is my snippet of code used for data processing
$ grep "Sample_ID" fang_et_al_genotypes.txt > fang_header.txt
$ grep -E "ZMMIL|ZMMLR|ZMMMR" fang_et_al_genotypes.txt > maize_genotypes.txt
$ cat fang_header.txt maize_genotypes.txt > maize_genotypes_header.txt
$ awk -f transpose.awk maize_genotypes_header.txt > transposed_maize_genotypes_header.txt
$ tail -n +4 transposed_maize_genotypes_header.txt > maize_geno.txt
$ sort -k1,1 maize_geno.txt > maize_sorted.txt
$ tail -n +2 snp_position.txt | cut -f 1,3,4 > snp_info_cut.txt
$ sort -k1,1 snp_info_cut.txt > snp_info_sorted.txt
$ join -1 1 -2 1 -t $'\t' snp_info_sorted.txt maize_sorted.txt > maize_snps.txt
$ awk '$2 == n' maize_snps.txt | grep -v "multiple" | sort -k3 -n > Result_Files/maize_n.txt
$ awk '$2 == n' maize_snps.txt | grep -v "multiple" | sort -k3 -n -r | sed 's/?/-/g' > Result_Files/maize_n_desc.txt
$ grep "unknown" maize_snps.txt > Result_Files/maize_unknown.txt
$ grep "multiple" maize_snps.txt > Result_Files/maize_multiple.txt
```

Here is my brief description of what this code does
extracts header from fang genotypes file to a new file
extracts maize genotype data to a new file
combines fang header and maize genotype data to new file
transposes extracted maize genotype data
removes header from maize genotype data for sorting, joining
sorts maize genotype data by SNP ID
extracts SNP_ID, chromosome number, position to new file for joining
sorts snp_info file by SNP ID for joining
joins snp info and genotype files for maize
command is run for each chromosome n, selects all SNPs on a chromosome, removes any entries with multiple positions on the same chromosome, sorts by position ascending, outputs to file for maize chromosome n in results folder
command run for each chromosome n, selects all SNPs on chromosome n, removes any entries with multiple positions on the same chromosome, sorts by position descending, missing information ?/? replaced with -/-, outputs to file in results folder
selects all SNPs of unknown position, outputs to result file
selects all SNPs with multiple positions, outputs to result file

###Teosinte Data

```
here is my snippet of code used for data processing
$ grep -E "ZMPBA|ZMPIL|ZMPJA" fang_et_al_genotypes.txt > teosinte_genotypes.txt
$ cat fang_header.txt teosinte_genotypes.txt > teosinte_genotypes_header.txt
$ awk -f transpose.awk teosinte_genotypes_header.txt > transposed_teosinte_genotypes_header.txt
$ tail -n +4 transposed_teosinte_genotypes_header.txt > teosinte_geno.txt
$ sort -k1,1 teosinte_geno.txt > teosinte_sorted.txt
$ join -1 1 -2 1 -t $'\t' snp_info_sorted.txt teosinte_sorted.txt > teosinte_snps.txt
$ awk '$2 == n' teosinte_snps.txt | grep -v "multiple" | sort -k3 -n > Result_Files/teosinte_n.txt
$ awk '$2 == n' teosinte_snps.txt | grep -v "multiple" |sort -k3 -n -r | sed 's/?/-/g' > Result_Files/teosinte_n_desc.txt
$ grep "unknown" teosinte_snps.txt > Result_Files/teosinte_unknown.txt
$ grep "multiple" teosinte_snps.txt > Result_Files/teosinte_multiple.txt
```

Here is my brief description of what this code does
extracts teosinte genotype data to a new file
transposes extracted teosinte genotype data
removes header from teosinte genortpe data
sorts teosinte genotype data by SNP ID for joining
joins snp info and genotype data for teosinte
command is run for each chromosome n, selects all SNPs on a chromosome, removes any entries with multiple positions on the same chromosome,sorts by position ascending, outputs to file for teosinte chromosome n in results folder
command run for each chromosome n, selects all SNPs on chromosome n, removes any entries with multiple positions on the same chromosome,sorts by position descending, missing information ?/? replaced with -/-, outputs to file in results folder
selects all SNPs of unknown position, outputs to result file
selects all SNPs with multiple positions, outputs to result file