# Part 1 Refine annotation

SSH:

```
ssh ms22568@bp1-login.acrc.bris.ac.uk
cd /user/work/ms22568/PF03722/
```


## Extend data
Extend the data, find from web database [InterPro](https://www.ebi.ac.uk/interpro/protein/UniProt/?ida=1c5ddad2f522955d19d44717600750d2a9ddb53d&search=Hexamerin&page_size=100#table), get the file: `reference_all_2041.fasta`, then use insecta_idxnum.txt to choose all insects seqs into another file named `reference_insect_1605.fasta`.

Then filter it, just need 3 types:
+ *Lepidoptera*
+ *Apis mellifera*
+ *Drosophila melanogaster*

Find them in `insecta.txt` and get needed idxnum, then use script:
```python
from Bio import SeqIO
import sys

def read_tax_ids(filename):
    ids = set()
    with open(filename, "r") as file:
        for line in file:
            ids.add(line.strip())
    return ids
def filter_fasta(input_fasta, ids, output_fasta):
    with open(input_fasta, "r") as input_file, open(output_fasta, "w") as output_file:
        for record in SeqIO.parse(input_file, "fasta"):
            header_parts = record.description.split(":")
            tax_id = header_parts[-1]
            if tax_id in ids:
                SeqIO.write(record, output_file, "fasta")

if __name__ == "__main__":
    input_fasta = "reference_all.fasta"
    tax_ids_file = "needed_idxnum.txt"
    output_fasta = "reference_needed.fa"
    tax_ids = read_tax_ids(tax_ids_file)
    filter_fasta(input_fasta, tax_ids, output_fasta)
```
Get `reference_needed_344.fasta` with 344 sequences.

> Note: Run again, found 368 needed sequences!

## Better data
Web CD research for `AllSpecies_PF03722.AA.fasta`

As Hemocyanin have 3 domains: N, M and C; search and find if the reads are complete or not, Fill a chart.

use CD search result, modify three domain into Hemocyanin_M/Hemocyanin_C/Hemocyanin_N, and as a `input.txt`.

Use the input file, make a table by a python script:
```python
import re
import csv
input_file = "C:/Study/Python/Project/annotation_table/input.txt"
output_file = "C:/Study/Python/Project/annotation_table/final_table.csv"

def parse_query_data(data):
    species = data.split('_')[1][:4]
    clade = re.split(r'->|_', data)[1]
    og = data.split('G')[2]
    scf = re.split(r'\.|G|\-' , data)[2]
    seq_id = scf + "G" + og
    return species, clade, og, scf, seq_id
def write_row_to_csv():
    species, clade, og, scf, seq_id = parse_query_data(prev_query)
    n_domain_length = int(n_data.get('To', 0)) - int(n_data.get('From', 0)) if n_data else ''
    m_domain_length = int(m_data.get('To', 0)) - int(m_data.get('From', 0)) if m_data else ''
    c_domain_length = int(c_data.get('To', 0)) - int(c_data.get('From', 0)) if c_data else ''
    new_row = [species, clade, f'Loc{locus_counter}', f'G{og}', seq_id, scf, '', '', '', '', '', '']
    if n_data:
        new_row.extend([n_data.get('Accession', ''), n_data.get('From', ''), n_data.get('To', ''), n_domain_length,
                        n_data.get('Incomplete', ''), n_data.get('Bitscore', ''), n_data.get('E-Value', '')])
    else:
        new_row.extend([''] * 7)
    if m_data:
        new_row.extend([m_data.get('Accession', ''), m_data.get('From', ''), m_data.get('To', ''), m_domain_length,
                        m_data.get('Incomplete', ''), m_data.get('Bitscore', ''), m_data.get('E-Value', '')])
    else:
        new_row.extend([''] * 7)
    if c_data:
        new_row.extend([c_data.get('Accession', ''), c_data.get('From', ''), c_data.get('To', ''), c_domain_length,
                        c_data.get('Incomplete', ''), c_data.get('Bitscore', ''), c_data.get('E-Value', '')])
    else:
        new_row.extend([''] * 7)
    for idx, ad in enumerate(additional_domains, start=1):
        ad_domain_length = int(ad.get('To', 0)) - int(ad.get('From', 0))
        new_row.extend([ad.get('Accession', ''),
                        ad.get('From', ''),
                        ad.get('To', ''),
                        ad_domain_length,
                        ad.get('Incomplete', ''),
                        ad.get('Bitscore', ''),
                        ad.get('E-Value', '')])
    remaining_cells = (max_additional_domains - len(additional_domains)) * 7
    new_row.extend([''] * remaining_cells)
    csvwriter.writerow(new_row)
def count_additional_domains(lines):
    count = 0
    current_count = 0
    for line in lines:
        if 'Hemocyanin' not in line:
            current_count += 1
        else:
            count = max(count, current_count)
            current_count = 0
    return max(count, current_count)
def create_additional_domain_headers(max_domains):
    headers = []
    for i in range(1, max_domains + 1):
        headers.extend([f'AD{i}_PFID', f'AD{i}_PF_Start', f'AD{i}_PF_end',
                        f'AD{i}_DomainLength', f'AD{i}_Incomplete',
                        f'AD{i}_bitscore', f'AD{i}_E-value'])
    return headers

with open(input_file, 'r') as infile, open(output_file, 'w', newline='') as outfile:
    lines = infile.readlines()
    headers = lines[0].strip().split()
    max_additional_domains = count_additional_domains(lines)
    additional_domain_headers = create_additional_domain_headers(max_additional_domains)
    csvwriter = csv.writer(outfile)
    csvwriter.writerow(['Species', 'Clade', 'Locus', 'OG', 'SeqID', 'Scf', 'Start', 'End', 'Strand', 'GenomicLength', 'Nexons', 'SeqLength',
                        'N_PFID', 'N_PF_Start', 'N_PF_end', 'N_DomainLength', 'N_Incomplete', 'N_bitscore', 'N_E-value',
                        'M_PFID', 'M_PF_Start', 'M_PF_end', 'M_DomainLength', 'M_Incomplete', 'M_bitscore', 'M_E-value',
                        'C_PFID', 'C_PF_Start', 'C_PF_end', 'C_DomainLength', 'C_Incomplete', 'C_bitscore', 'C_E-value',
                        ]+ additional_domain_headers)
    locus_counter = 1
    n_data = m_data = c_data = None
    additional_domains = []
    prev_query = None
    for line in lines[1:]:
        row = dict(zip(headers, line.strip().split()))
        short_name = row['Short_name']
        query = row['Query']
        if query != prev_query and prev_query is not None:
            write_row_to_csv()
            locus_counter += 1
            n_data = m_data = c_data = None
            additional_domains = []
        prev_query = query
        if 'Hemocyanin_N' in short_name:
            n_data = row
        elif 'Hemocyanin_M' in short_name:
            m_data = row
        elif 'Hemocyanin_C' in short_name:
            c_data = row
        else:
            additional_domains.append(row)
    write_row_to_csv()
```

`final_table.csv` -> `final_table.xlsx`

In input file, some sequences have multiple M/N/C domains, find them:
```python
input_file = "C:/Study/Python/Project/annotation_table/input.txt"
output_file = "C:/Study/Python/Project/annotation_table/multi_Hemo.txt"

with open(input_file, "r") as f:
    lines = f.readlines()
results = {}
for line in lines:
    if line.startswith("Query"):
        pass
    else:
        parts = line.split("\t")
        query_id = parts[0].split('-')[0]
        short_name = parts[8]
        if query_id not in results:
            results[query_id] = []
        results[query_id].append(short_name)
filtered_results = {}
for query_id, hemocyanin_list in results.items():
    hemocyanin_count = {"Hemocyanin_M": 0, "Hemocyanin_C": 0, "Hemocyanin_N": 0}
    for hemocyanin in hemocyanin_list:
        if hemocyanin in hemocyanin_count:
            hemocyanin_count[hemocyanin] += 1
    if any(value > 1 for value in hemocyanin_count.values()):
        filtered_results[query_id] = hemocyanin_list 
with open(output_file, "w") as f:
    f.write("Query\tHemocyanin\n")
    for query_id, hemocyanin_list in filtered_results.items():
        hemocyanin_str = ", ".join(hemocyanin_list)
        f.write(f"{query_id}\t{hemocyanin_str}\n")
```

That could lead some questions, find them in `final_table.xlsx`, manually fix them and highlight.

And need to map aa to nt
```bash
cd /user/work/ms22568/PF03722/aa_to_nt
exonerate="../../../tk19812/software/exonerate/bin/exonerate"
$exonerate --percent 95 --model protein2genome --query aa.fasta --target nt.fasta > map.out
```

write script to auto do this:
```shell
#!/bin/bash

file1="AllSpecies_fixed_grey&green.fasta"
file2="AllSpecies_PF03722.NT.fasta"
outfile="All.out"
exonerate_path="../../../tk19812/software/exonerate/bin/exonerate"
# Clear the output file
> $outfile
# Retrieve unique headers from file1 (maintaining order)
headers=$(grep "^>" $file1 | cut -d ' ' -f1 | awk '!seen[$0]++')
# Iterate over each unique header
for header in $headers; do
    echo $header
    NTheader=`echo $header | sed 's/_gene/ /' | cut -f1 -d' '`
    grep -A1 -w "${header}" $file1 > temp1.fasta
    grep -A1 -w "${NTheader}" $file2 > temp2.fasta

    # If temp2.fasta is not empty, run exonerate on temp files and append to outfile
    if [ -s temp2.fasta ]; then
        $exonerate_path --percent 95 --model protein2genome --query temp1.fasta --target temp2.fasta >> $outfile
        echo "OK for header: $header"
    else
        echo "No sequences found for base header: $header"
    fi
done
# Clean up temp files
rm temp1.fasta temp2.fasta
```

get `All.out` file, will be like:
```
C4 Alignment:
------------
         Query: Agraulis_Avcr.Avcr1713G12653.1_gene=Avcr.Avcr1713G12653
        Target: Agraulis_Avcr.Avcr1713G12653.1 gene=Avcr.Avcr1713G12653
         Model: protein2genome:local
     Raw score: 3943
   Query range: 0 -> 750
  Target range: 0 -> 2250

    1 : MetLysAlaIleValCysLeuValLeuGlyLeuThrAlaIleAlaAlaAlaArgProAspVa :   21
        ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
        MetLysAlaIleValCysLeuValLeuGlyLeuThrAlaIleAlaAlaAlaArgProAspVa
    1 : ATGAAGGCAATAGTGTGCTTAGTCTTAGGGTTAACGGCTATTGCCGCGGCCCGCCCCGACGT :   61

```

extract nt sequences using python:
```python
def extract_sequences(filename):
    with open(filename, 'r') as file:
        data = file.read()
        blocks = data.split('------------')
        for block in blocks[1:]:  # skipping the first block which contains command line info
            lines = block.strip().split('\n')
            query = lines[0].replace('Query: ', '')
            sequences = []
            for i in range(1, len(lines)):
                if '|' in lines[i]:
                    sequence_line = lines[i+2]
                    # Extract the sequence part of the line
                    sequence = re.search(r': .* :', sequence_line)
                    if sequence:
                        sequence = sequence.group().split(' ')[1]
                        sequences.append(sequence)  # use group(1) to extract the DNA sequence
            sequence = ''.join(sequences)
            if len(sequence) % 3 != 0:
                print(query)
            yield '>' + query + '\n' + sequence + '\n'
```

deduplicate and fix some unmatched gap like:
```
Query: Doris_Hdor.Hdor0210G156.1_gene=Hdor.Hdor0210G156
  415 : hrLysLeuProProTyrGluArgAsp{Ar}  >>>> Target Intron 1 >>>>  {g} :  423
        ||||||||||||||||||||||||||{!!}            798 bp           {!}
        hrLysLeuProProTyrGluArgAsp{Se}+-                         +-{r}
 1241 : CTAAATTACCACCTTATGAAAGGGAT{AG}gc.........................at{T} : 2067

Query: Doris_Hxan.Scf0000126G22334.1_gene=Hxan.Scf0000126G22334
415 : roTyrGluArgAspArg{Ar}  >>>> Target Intron 1 >>>>  {g}LeuAspPhe :  423
        |||||||||||||||||{||}            795 bp           {|}|||||||||
        roTyrGluArgAspArg{Ar}-+                         +-{g}LeuAspPhe
 1253 : CTTATGAAAGGGATAGG{CG}tt.........................at{T}TTGGATTTC : 2074
```

Now got the nt and aa fastas:
+ `AllSpecies_aa_formatted.fasta`
+ `AllSpecies_nt_formatted.fasta`

They should have the same order and header.

```python
def check_fasta_headers(file1, file2):
    with open(file1, 'r') as f1, open(file2, 'r') as f2:
        headers1 = [line.strip() for line in f1 if line.startswith('>')]
        headers2 = [line.strip() for line in f2 if line.startswith('>')]
    return headers1 == headers2

file1 = 'C:/Study/Python/Project/annotation_table/phylogenetic_tree/AllSpecies_aa_formatted.fasta'
file2 = 'C:/Study/Python/Project/annotation_table/phylogenetic_tree/AllSpecies_nt_formatted.fasta'
same_headers = check_fasta_headers(file1, file2)
if same_headers:
    print("OK")
else:
    print("No")
```

## Phylogenetic tree 

Firstly, need to handle to extended data, as the are the reference data.

One-liner:
```python
def reformat_fasta(fasta_file, output_file):
    with open(fasta_file, 'r') as infile, open(output_file, 'w') as outfile:
        lines = infile.readlines()
        for i, line in enumerate(lines):
            if line.startswith('>'):
                # If it's not the first line, add a newline character before the header
                if i != 0:
                    outfile.write('\n')
                outfile.write(line)
            else:
                outfile.write(line.strip())
        # Add a final newline at the end of the file
        outfile.write('\n')
```

Change header:
```python
def reformat_headers(fasta_file, output_file):
    with open(fasta_file, 'r') as infile, open(output_file, 'w') as outfile:
        lines = infile.readlines()
        for line in lines:
            if line.startswith('>'):
                parts = line[1:].strip().split('|')  # remove leading '>' and split on '|'
                species = parts[2]
                new_species = species.split('_')[0][0] + species.split('_')[1][:3] + '.'  # first letter of first word + first three letters of second word
                new_header = f'>{new_species}{parts[0]}_{parts[1].replace(" ", "_")}\n'  # assemble new header
                outfile.write(new_header)
            else:
                outfile.write(line)
```

Then shorten the gene name, like:
+ Tyrosinase_copper-binding_domain-containing_protein into Tyrosinase_CBDC_protein or prophenoloxidase
+ storage_protein into SP
+ Methionine into Met 
+ basic_juvenile_hormone-suppressible_protein_2 into basic_JHSP2
+ Larval_serum_protein_2 into acidic_JHSP1
+ subunit to sub
and so on

Use [Clustal Omega](https://www.ebi.ac.uk/Tools/msa/clustalo/) do multiple alignment.

Then use fasttree to make a tree first, see if there are any problems, according to it, fix the alignment.


When complete, use IQtree to make a more rubust tree.
```shell
#!/bin/bash
#SBATCH -J iqtree
#SBATCH --account=GELY028645
#SBATCH --nodes=1
#SBATCH --mem=100G
#SBATCH --ntasks=4

module load apps/iqtree/2.2.0.3
cd /user/work/ms22568/PF03722/phylogenetic_tree/
iqtree2 -B 5000 -s aln.fasta -nt AUTO -m MFP --keep-ident

```


## Annotate and check copy number



Mapping 

```shell
/user/work/tk19812/software/miniprot-0.11_x64-linux/miniprot --aln --trans --gff -t4 Hpet.assembly.v0.9.fasta Hpet_map_seq.fas > Hpet_output.gff
```
