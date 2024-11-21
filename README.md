# BIO-312-extra-credit
Lab 3
```ncbi-acc-download -F fasta -m protein "NP_859076.3"
This command downlaods the query sequence 
blastp -db ../allprotein.fas -query NP_859076.3.fa -outfmt 0 -max_hsps 1 -out mygene.blastp.typical.out,less globins.blastp.typical.out
This command performs the BLAST search
blastp -db ../allprotein.fas -query NP_859076.3.fa  -outfmt "6 sseqid pident length mismatch gapopen evalue bitscore pident stitle"  -max_hsps 1 -out mygene.blastp.detail.out
This command performs a BLAST search, and request tabular output
awk '{if ($6< 1e-30)print $1 }' mygene.blastp.detail.out > mygene.blastp.detail.filtered.out
This command filters the BLAST output for high-scoring putative homologs
wc -l mygene.blastp.detail.filtered.out
Counts the amount of BLAST hits 
grep -o -E "^[A-Z]\.[a-z]+" mygene.blastp.detail.filtered.out  | sort | uniq -c
Counts the amount of paralogs```


Lab 4 
seqkit grep --pattern-file ~/lab03-$MYGIT/globins/globins.blastp.detail.filtered.out ~/lab03-$MYGIT/allprotein.fas | seqkit grep -v -p "carpio" > ~/lab04-$MYGIT/globins/globins.homologs.fas
obtain the sequences that are in the BLAST output file from lab 3
muscle -align ~/lab04-$MYGIT/mynewgene/mynewgene.homologs.fas -output ~/lab04-$MYGIT/mynewgene/mynewgene.homologs.al.fas
make a multiple sequence alignment using muscle
 Rscript --vanilla ~/lab04-$MYGIT/plotMSA.R  ~/lab04-$MYGIT/mynewgene/mynewgene.homologs.al.fas
 print your alignment to a large pdf file
alignbuddy  -al  ~/lab04-$MYGIT/mynewgene/mynewgene.homologs.al.fas
calculate the width of alignment
alignbuddy -trm all  ~/lab04-$MYGIT/mynewgene/mynewgene.homologs.al.fas | alignbuddy  -al
length of the alignment after removing any column with gaps
alignbuddy -dinv 'ambig' ~/lab04-$MYGIT/mynewgene/mynewgene.homologs.al.fas | alignbuddy  -al
 length of the alignment after removing invariant positions
 t_coffee -other_pg seq_reformat -in ~/lab04-$MYGIT/mynewgene/mynewgene.homologs.al.fas -output sim
 average percent identity using t_coffee
  alignbuddy -pi ~/lab04-$MYGIT/globins/globins.homologs.al.fas | awk ' (NR>2)  { for (i=2;i<=NF  ;i++){ sum+=$i;num++} }
     END{ print(100*sum/num) } '
      average percent identity using alignbuddy.```
      Lab 5
      sed 's/ //g' ~/lab04-$MYGIT/mynewgene/mynewgene.homologs.al.fas | seqkit grep -v -r -p "dupelabel" > ~/lab05-$MYGIT/mynewgene/NP_859076.3.homologsf.al.fas 
      remove any sequence that contains a duplicate label tag
iqtree -s ~/lab05-$MYGIT/mynewgene/NP_859076.3.homologsf.al.fas -bb 1000 -nt 2
      find the maximum likehood tree estimate
 nw_display ~/lab05-$MYGIT/mynewgene/NP_859076.3.homologsf.al.fas.treefile
      To display the graphic 
      Rscript --vanilla ~/lab05-$MYGIT/plotUnrooted.R  ~/lab05-$MYGIT/mynewgene/NP_859076.3.homologsf.al.fas.treefile ~/lab05-$MYGIT/mynewgene/NP_859076.3.homologsf.al.fas.treefile.pdf 0.4 15
       look at it unrooted with a graphical display
gotree reroot midpoint -i ~/lab05-$MYGIT/mynewgene/NP_859076.3.homologsf.al.fas.treefile -o ~/lab05-$MYGIT/mynewgene/NP_859076.3.homologsf.al.mid.treefile
       Reroot the tree 
nw_order -c n ~/lab05-$MYGIT/mynewgene/NP_859076.3.homologsf.al.mid.treefile  | nw_display -      
look at the rooted tree at the command line
nw_order -c n ~/lab05-$MYGIT/mynewgene/NP_859076.3.homologsf.al.mid.treefile | nw_display -w 1000 -b 'opacity:0' -s  >  ~/lab05-$MYGIT/globins/globins.homologsf.al.mid.treefile.svg -
Make it into a graphic 
nw_order -c n ~/lab05-$MYGIT/globins/globins.homologsf.al.mid.treefile | nw_topology - | nw_display -s  -w 1000 > ~/lab05-$MYGIT/globins/globins.homologsf.al.midCl.treefile.svg -
convert ~/lab05-$MYGIT/globins/globins.homologsf.al.midCl.treefile.svg ~/lab05-$MYGIT/globins/globins.homologsf.al.midCl.treefile.pdf
Switches it from a phylogram to a cladogram
