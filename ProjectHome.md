

# Latest news (Version 0.5.3, 20-Aug-2010) #
  1. Vastly improved version of our multiple-mapping deduping script (dedupDiscordants.py).  This greatly improves breakpoint calling specificity.
  1. New version of pairDiscordants.py that resolves several minor bugs.
  1. Fixed minor bug in Hydra that occassionally caused false positive calls from readpairs that did not actually support a common breakpoint.
  1. Updated the example [workflow](http://code.google.com/p/hydra-sv/wiki/TypicalWorkflow) page to reflect changes in our pipeline.


# Hydra Summary #
Hydra detects structural variation (SV) breakpoints by clustering **discordant** paired-end alignments whose "signatures" corroborate the same putative breakpoint. Hydra can detect breakpoints caused by **all classes of structural variation**. Moreover, it was designed to detect variation in both **unique and duplicated genomic regions**; therefore, it will examine paired-end reads having multiple discordant alignments.

It is important to note that Hydra does not attempt to **_classify_** SV breakpoints based on the mapping distances and orientations of each breakpoint cluster.  In other words, it merely detects and reports breakpoints, but does not decide what type of SV (e.g. deletion, inversion, etc.) is indicated by the apparent breakpoint. This is an intentional decision, as we have observed that in loci affected by complex rearrangements, the type of variant suggested by the breakpoint signature is not always correct. Hydra does report the orientations, distances, number of supporting read-pairs, etc., for each breakpoint.  We suggest that downstream methods be used to classify variants based on the genomic features that they overlap and the co-occurrence of other breakpoints.  For example, we developed [BEDTools](http://bedtools.googlecode.com) for exactly this purpose and the breakpoints reported by Hydra are in the [BEDPE](http://code.google.com/p/bedtools/#BEDPE_%28Paired-end_BED%29_format) format used by BEDTools. Future releases of Hydra will include scripts that assist in the classification process.

Hydra was developed by **Aaron Quinlan** and **Ira Hall** at the University of Virginia.  It is written in C++ and is under continued development. **Therefore, please check back frequently, as we will continue to update the source code and documentation as Hydra and the methods leading up to it (e.g. aligners, alignment formats, sequence technologies and protocols) evolve.**

<br>

<h1>Software in Hydra suite</h1>
<table><thead><th> <b>Program</b> </th><th> <b>Description</b> </th></thead><tbody>
<tr><td> <i>hydra</i>   </td><td> Calls SV breakpoints. </td></tr>
<tr><td> <i>bamToFastq</i> </td><td> Creates a FASTQ file from alignments in BAM file. </td></tr>
<tr><td> <i>bamToBed</i> </td><td> Available via the <a href='http://bedtools.googlecode.com'>BEDTools</a> package. </td></tr>
<tr><td> <i>pairToBed</i> </td><td> Compares paired-end alignments in BEDPE format to genome features in BED format.  Allows one to, for example, remove mappings where both ends are in SSRs / VNTRs. Available via the <a href='http://bedtools.googlecode.com'>BEDTools</a> package.</td></tr>
<tr><td> <i>pairDiscordants.py</i> </td><td> Creates all pairing possibilities for discordant mappings from each each of a read-pair.</td></tr>
<tr><td> <i>dedupDiscordants.py</i> </td><td> Collapses duplicate read-pair mappings into a single, best mapping. </td></tr></tbody></table>


<br>
<h1>Installation</h1>
<pre><code>tar -zxvf Hydra.&lt;version&gt;.tar.gz <br>
cd Hydra<br>
make clean # in case I forgot to clear the binaries<br>
make all<br>
ls bin<br>
<br>
# copy the binaries to a directory in your PATH. e.g., <br>
sudo cp bin/* /usr/local/bin<br>
sudo cp scripts/* /usr/local/bin<br>
</code></pre>

<br>

<h1>Documentation</h1>
<ul><li><a href='http://code.google.com/p/hydra-sv/wiki/FileFormats'>File formats</a>
</li><li><a href='http://code.google.com/p/hydra-sv/wiki/TypicalWorkflow'>Suggested workflow</a>
</li><li><a href='http://code.google.com/p/hydra-sv/wiki/Glossary'>Glossary</a></li></ul>

We will continue to update and improve the documentation over the coming weeks/months.  In the interim, please refer to the Methods and Supplementary Methods sections in the following Genome Research article.<br>
<br>
<br>

<h1>Caveats</h1>
As with all SV detection methods based on current paired-end DNA sequencing technologies, the quality/accuracy of the detected breakpoints greatly depends on the quality of the sequence alignments leading up to calling breakpoints.  In many ways, it is a classic "garbage in, garbage out" problem.  Therefore, we strongly suggest that users of Hydra carefully scrutinize the discordant alignments that are used as input to Hydra.  Try to be certain that you have used aligners and settings that can detect essentially all concordant read-pairs, even if they have SNPs, sequencing errors or INDELs that would otherwise perturb their discovery using a naive aligner with "fast" alignment settings.<br>
<br>
<br>

<h1>Ongoing work</h1>
<ol><li>Direct support for BAM input files.<br>
</li><li>Split-read SV detection.<br>
</li><li><i>de novo</i> breakpoint assembly.<br>
</li><li>Breakpoint sequence annotation; mechanistic inference.</li></ol>

<br>

<h1>Contact</h1>
<ul><li>Aaron Quinlan, University of Virginia. firstlast (at) gmail dot com.<br>
</li><li>Ira Hall, University of Virginia.