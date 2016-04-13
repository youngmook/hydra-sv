<br>

<b>Overview.</b>
<br>
The pipeline described below is meant to convey a general approach that we have found to be effective for discovering SV breakpoints with Hydra. However, it should be noted that this approach seeks to discover breakpoints arising from both "unique" <b>and duplicated</b> (e.g., segmental duplications, recent retrotransposons).  Therefore, it includes a final step (Tier 3) that attempts to capture <b>mappings</b> for readpairs that align to such duplicated sequences.  Hydra then uses those mappings to search for transposon insertions and rearrangements among segmental duplications.  If you are not interested in these types of events, you can eliminate the Tier 3 alignment step.<br>
<br>
<br>
<br>

<b>Why tiered alignment?</b>
<br>
We employ a tiered alignment strategy in order to optimize the tradeoff between alignment speed and sensitivity.  Clearly, we could just skip the first two tiers described below and simply align all readpairs with the extremely sensitive settings for Tier 3.  The problem is that this can take an extremely long time for large datasets.  A tiered alignment approach begins with a fast aligner to quickly identify "proper pairs" or "concordant pairs".  We then scrutinize the remaining "improper or discordant" pairs with a more sensitive aligner.  The motivation here is that the aligner from the first tier didn't have the sensitivity to identify that a discordant is actually concordant alignments.<br>
<br>
<br>

<b>A note on aligners.</b>
<br>
The pipeline below is based upon BWA and Novoalign, but is not meant to suggest that these are the only tools that are possible to use for SV analysis with Hydra.  For tier 1, other aligners such as Bowtie or Mosaik could easily be used.  For tier 2 alignment, we suggest a slower, hash-based aligner using a small hash/kmer size (e.g. 12-14 bp), as you want this step to be more sensitive so that you can remove concordants that were missed in tier 1.  Mosaik and BFAST be good alternatives here.  As for tier 3, we have found that very few aligners will actually report multiple mappings for discordant pairs in SAM format.  This is a requirement for Hydra in order to detect SV in duplicated sequence.  Our experience is that Novoalign works well.  The downside is that the freely-available version of Novoalign is rather slow.  We are looking into the use of MrFast as a freely-available alternative.<br>
<br>
<br>


<h1>An example pipeline.</h1>
The "$" denotes the command line prompt.<br>
<br>
<b>Tier 1 alignment</b>. This initial alignet step is meant to quickly identify (and remove) concordant readpairs from your alignment. Align your paired-end reads to the reference genome with a fast and reasonably sensitive aligner (e.g.,  <a href='http://bio-bwa.sourceforge.net/'>BWA</a> or <a href='http://bowtie-bio.sourceforge.net/index.shtml'>Bowtie</a>). Here is an example for the BWA aligner against hg18 (assuming you have already created a BWT index of the genome).  We will create a BAM file by piping the BWA output directly to samtools.<br>
<br>
<b>Note:</b> After tier1 alignment, one should compute the fragment size distribution of your library from the "proper/concordant" pairs.  These statistics (median and stdev or MAD) will be used by Hydra to know the variance of discordant pairs that support the same breakpoint.<br>
<br>
<br>
<pre><code>$ bwa aln -t &lt;THREADS&gt; hg18 sample.1.fq &gt; sample.1.sai<br>
$ bwa aln -t &lt;THREADS&gt; hg18 sample.2.fq &gt; sample.2.sai<br>
$ bwa sampe hg18 sample.1.sai sample.2.sai  \<br>
                 sample.1.fq  sample.2.fq | \<br>
      samtools view -Sb - &gt; sample.tier1.queryorder.bam <br>
</code></pre>

<br>

<b>Tier 2 alignment</b>. Grab the discordant alignments from the <b>tier 1</b> BAM files and create FASTQ files for the discordant pairs.  Align the tier 1 discordant pairs with a more sensitive aligner such as <a href='http://www.novocraft.com'>Novoalign</a> or <a href='http://code.google.com/p/mosaik-aligner/'>Mosaik</a> using a word size of no more than 15bp (we typically use 11-14 with Novoalign at this stage). We find this to be a necessary step as many concordant pairs are missed in the first tier (a consequence of the tradeoff b/w speed and senitivity). In turn, these missed concordant pairs lead to false positive SV calls. At this point, you are merely trying to eliminate remaining concordant pairs.  Thus, if using Novoalign, you should use the "-r Random" alignment mode.  Below is an example for creating discordant FASTQ files from the tier1 discordant alignments and then aligning those reads to hg18 with Novoalign.  This assumes we have created an index of hg18 with novoindex using kmer size 14 and step size 1.  Let's assume that in step 1, you determine that your fragment library has a median of 500 and a standard deviation of 50.<br>
<br>
<pre><code>$ samtools view -uF 2 sample.tier1.queryorder.bam | \<br>
           bamToFastq -bam stdin \<br>
                      -fq1 sample.tier1.disc.1.fq \<br>
                      -fq2 sample.tier1.disc.2.fq<br>
<br>
$ novoalign -c &lt;THREADS&gt; -d hg18.k14s1 \<br>
                         -f sample.tier1.disc.1.fq sample.tier1.disc.2.fq \<br>
                         -i 500 50 -r Random -o SAM | \<br>
            samtools view -Sb - &gt; sample.tier2.queryorder.bam <br>
<br>
</code></pre>

<br>

<b>Tier 3 alignment</b>.  Grab the discordant alignments from the <b>tier 2</b> BAM files and create FASTQ files for the discordant pairs. Align the tier 2 discordant pairs with the same aligner from step 2 (or a more sensitive tool if you have the CPU cycles).  In this stage, however, we want to track <b><i>all</i></b> discordant alignment positions and use even more sensitive parameters such that any remaining "crypic" concordant pairs are detectable.  Retaining multiple mapping positions for the discordant pairs will allow us to detect SV involving duplicated/repetitive sequences such as segmental duplications and transposable elements.  Clearly this is an optional step: if you don't care about SV in duplicated sequence, only retain uniquely aligned reads---Hydra will work nonetheless.  If possible (i.e, if you have a sufficient number of CPUs), you should decrease the word size used for alignment to further eliminate remaining concordant pairs that may exist solely because a larger alignment seed was previously missed owing to sequence errors and/or polymorphism.  If using Novoalign, we recommend the following settings: -rE and -t at least 180, perhaps up to 300.<br>
<pre><code>$ samtools view -uF 2 sample.tier2.queryorder.bam | \<br>
           bamToFastq -bam stdin \<br>
           -fq1 sample.tier2.disc.1.fq \<br>
           -fq2 sample.tier2.disc.2.fq<br>
<br>
$ novoalign -c &lt;THREADS&gt; -d hg18.k14s1 \<br>
                         -f sample.tier2.disc.1.fq sample.tier2.disc.2.fq \<br>
                         -i 500 50 -r Ex 1100 -t 300 -o SAM | \<br>
            samtools view -Sb - &gt; sample.tier3.queryorder.bam <br>
<br>
</code></pre>


<br>

<b>Create all discordant mapping combinations for each discordant read-pair.</b> Use the pairDiscordants.py script to screen for remaining concordant pairs and for all discordant pairs, create the mapping combinations for SV detection with Hydra.  pairDiscordants.py allows you to set the upper (-z) and lower (-y) bound for concordants (this should be derived by plotting the alignment distance of your pairs and computing median and m.a.d. values).  It also allows you to set the maximum number of mapping positions allowed for a given discordant pair.  We typically set this to 1000 (-n).  Use the "-m hydra" option to create output that is appropriate for use with Hydra. The other options used for pairDiscordants are based on the statistic of your library.  The numbers used here are based on the statistics computed in tier 1.<br>
<br>
<pre><code>$ bamToBed -i sample.tier3.queryorder.bam -tag NM | \<br>
           pairDiscordants.py -i stdin \<br>
                              -m hydra \<br>
                              -z 800 &gt; sample.disc.bedpe<br>
</code></pre>

<br>


<b>Remove PCR and sequencing duplicates.</b>
<pre><code>$ dedupDiscordants.py -i sample.disc.bedpe -s 3 &gt; sample.disc.deduped.bedpe<br>
</code></pre>

<br>


<b>Detect SV breakpoints with Hydra</b>  The -mld and -mno values are based on the statistics computed in tier 2.  Use the Hydra help (-h) to find suggested approaches for computing the values for these parameters.<br>
<pre><code>$ hydra -in sample.disc.deduped.bedpe -out sample.breaks -mld 500 -mno 1500<br>
</code></pre>

<br>

<b>Characterize/classify Hydra breakpoints with <a href='http://code.google.com/p/bedtools'>BEDTools</a>, Galaxy, UCSC Genome Browser, etc.</b> For example, use transposon annotations to distinguish true deletions in an experimental genome versus transposon insertions in the reference genome.