# 1
test@ca2f5624c40d:~/linux$ cat 1.gtf | awk '$1 =="XI" && $3 =="CDS" { print $0 } ' | sort -k5 | tail
XI      ensembl CDS     82947   82998   .       +       0       gene_id "YKL190W"; gene_version "1"; transcript_id "YKL190W"; transcript_version "1"; exon_number "1"; gene_name "CNB1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "CNB1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKL190W"; protein_version "1";
XI      ensembl CDS     83075   83547   .       +       1       gene_id "YKL190W"; gene_version "1"; transcript_id "YKL190W"; transcript_version "1"; exon_number "2"; gene_name "CNB1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "CNB1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKL190W"; protein_version "1";
XI      ensembl CDS     84704   85900   .       +       0       gene_id "YKL189W"; gene_version "1"; transcript_id "YKL189W"; transcript_version "1"; exon_number "1"; gene_name "HYM1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "HYM1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKL189W"; protein_version "1";
XI      ensembl CDS     86228   88786   .       -       0       gene_id "YKL188C"; gene_version "1"; transcript_id "YKL188C"; transcript_version "1"; exon_number "1"; gene_name "PXA2"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "PXA2"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKL188C"; protein_version "1";
XI      ensembl CDS     89287   91536   .       -       0       gene_id "YKL187C"; gene_version "1"; transcript_id "YKL187C"; transcript_version "1"; exon_number "1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "YKL187C"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKL187C"; protein_version "1";
XI      ensembl CDS     92747   93298   .       -       0       gene_id "YKL186C"; gene_version "1"; transcript_id "YKL186C"; transcript_version "1"; exon_number "1"; gene_name "MTR2"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "MTR2"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKL186C"; protein_version "1";
XI      ensembl CDS     94499   96262   .       +       0       gene_id "YKL185W"; gene_version "1"; transcript_id "YKL185W"; transcript_version "1"; exon_number "1"; gene_name "ASH1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "ASH1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKL185W"; protein_version "1";
XI      ensembl CDS     96757   98154   .       +       0       gene_id "YKL184W"; gene_version "1"; transcript_id "YKL184W"; transcript_version "1"; exon_number "1"; gene_name "SPE1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "SPE1"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKL184W"; protein_version "1";
XI      ensembl CDS     98398   98607   .       -       0       gene_id "YKL183C-A"; gene_version "1"; transcript_id "YKL183C-A"; transcript_version "1"; exon_number "1"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "YKL183C-A"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKL183C-A"; protein_version "1";
XI      ensembl CDS     98721   99638   .       +       0       gene_id "YKL183W"; gene_version "1"; transcript_id "YKL183W"; transcript_version "1"; exon_number "1"; gene_name "LOT5"; gene_source "ensembl"; gene_biotype "protein_coding"; transcript_name "LOT5"; transcript_source "ensembl"; transcript_biotype "protein_coding"; protein_id "YKL183W"; protein_version "1";

# 2
test@ca2f5624c40d:~/linux$ grep -v '^#' 1.gtf | awk ' $1 =="IV" { print $3 } ' | sort | uniq -c | sort -k1
    853 start_codon
    853 stop_codon
    886 gene
    886 transcript
    895 CDS
    933 exon

# 3
test@ca2f5624c40d:~/linux$ cat 1.gtf | awk ' $1 !="IV" && $3 =="CDS" && $7 =="-" { print $5-$4 + 1 } ' | sort | tail -2
999
999

# 4
test@ca2f5624c40d:~/linux$ awk -F'\t' '$1 == "XV" && $3 == "gene" {split($9,a,"gene_id \""); split(a[2],b,"\""); len=$5-$4+1; print b[1], len}' 1.gtf | sort -k2nr | head -n5
YOL081W 9240
YOR396W 5391
YOR192C-B 5314
YOR343W-B 5314
YOL103W-B 5269

# 5
test@ca2f5624c40d:~/linux$ grep -v '^#' 1.gtf | awk -F'\t' '{print NF}' | sort -u
9
