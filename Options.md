**Usage:**
```
$ hydra [OPTIONS] -in <mappings> -out <breakpoints> -mld <bp> -mno <bp>
```

**Required:**

| -in	| Input file of discordant mappings. [Details.](http://code.google.com/p/hydra-sv/wiki/FileFormats#Hydra_input_file_format)|
|:----|:-------------------------------------------------------------------------------------------------------------------------|
| -out	| Stub for Hydra output files. [Details.](http://code.google.com/p/hydra-sv/wiki/FileFormats#Hydra_output_files)           |
| -mld	|Maximum allowable length difference b/w mappings. Typically set to 10 X m.a.d. of the DNA fragment libraries.             |
| -mno	| Maximum allowable non-overlap b/w mappings. Typically set to median + (20 X m.a.d.) of the DNA fragment libraries.       |


**Options:**

| -ms | Minimum number of pairs required for variant to be called. **Default: 2**|
|:----|:-------------------------------------------------------------------------|
| -lnk | Maximum intrachromosomal distance allowed before a variant is considered to be between unlinked DNA segments. **Default: 1000000 (i.e., 1Mb)**|
| -is | Choose most likely variant (when a tie exists) based on least edit distance rather than size.|
| -li | Combine +/+ and -/- mappings when screening for inversions. This increases inversion detection sensitivity in low coverage.|
| -use | Which mappings should be used for each pair? **"best"** Use the mappings with the least edit distance for each pair (Default). **"all"** Use all mappings for each pair. **N** Use the best plus those within N edit distance of best.|