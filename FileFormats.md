

<br>
<h1>Hydra input file format</h1>
Hydra currently expects a <b>tab-delimited</b> input text file describing that includes <b>only discordant paired-end mappings</b>.  Future versions will ultimately be based on SAM/BAM input files.<br>
<br>
<b>NOTE: The input file must be sorted (or just grouped) by the name column (#7).</b>

<table><thead><th> <b>Field Nbr.</b> </th><th> <b>Field Name</b> </th><th> <b>Description</b> </th></thead><tbody>
<tr><td> 1.                </td><td> chrom1            </td><td> Chromosome for end 1 </td></tr>
<tr><td> 2.                </td><td> start1            </td><td> Start position for end 1 </td></tr>
<tr><td> 3.                </td><td> end1              </td><td> End position for end 1 </td></tr>
<tr><td> 4.                </td><td> chrom2            </td><td> Chromosome for end 2 </td></tr>
<tr><td> 5.                </td><td> start2            </td><td> Start position for end 2 </td></tr>
<tr><td> 6.                </td><td> end1              </td><td> End position for end 2 </td></tr>
<tr><td> 7.                </td><td> name              </td><td> Name of the pair   </td></tr>
<tr><td> 8.                </td><td> mate1End          </td><td> To which mate of the pair do fields 1,2,3,9,11 correspond? (values: 1 or 2) </td></tr>
<tr><td> 9.                </td><td> strand1           </td><td> Orientation for end 1 (+ or -) </td></tr>
<tr><td> 10.               </td><td> strand2           </td><td> Orientation for end 2 (+ or -) </td></tr>
<tr><td> 11.               </td><td> editDist1         </td><td> Alignment edit distance for end 1 (can be extracted from NM tag in SAM) </td></tr>
<tr><td> 12.               </td><td> editDist2         </td><td> Alignment edit distance for end 2 (can be extracted from NM tag in SAM) </td></tr></tbody></table>

<b>Example input file data:</b>
<pre><code>chr2    144494477       144494519       chr2    144495361       144495403       113100100881914    2       +       -       0       0       <br>
chr16   91448892        91448934        chr11   21386025        21386067        113100100870284    1       +       -       0       0       <br>
chr8    125953105       125953147       chr8    125954583       125954625       113100100781853    2       +       -       0       1       <br>
chr1    133543151       133543193       chr8    125953105       125953147       113100100781853    1       +       +       1       0       <br>
chr5    98625244        98625286        chrX    7971907 7971949 113100100750512    1       -       +       0       0       <br>
chr5    77697526        77697568        chr5    77704261        77704303        113100101640929    1       +       -       1       0       <br>
chr5    41154266        41154308        chr3    76024969        76025011        113100101661135    1       -       +       0       0       <br>
chr11   9219002 9219044 chr1    158568331       158568373       113100101751675    1       -       -       0       0       <br>
chr2    109719983       109720025       chr10   14800365        14800407        113100100411933    1       +       +       0       0       <br>
chr2    109719983       109720025       chr10   14805392        14805434        113100100411933    1       +       +       0       0       <br>
chr2    109719983       109720025       chr10   21456086        21456128        113100100411933    1       +       +       0       0       <br>
chr2    109719983       109720025       chr10   69791656        69791698        113100100411933    1       +       +       0       0<br>
</code></pre>


<br>
<h1>Hydra output files</h1>
The first 10 columns of all Hydra output files are in the <a href='http://code.google.com/p/bedtools/wiki/FileFormats#BEDPE_%28Paired-end_BED%29_format'>BEDPE</a> format used with the <a href='http://code.google.com/p/bedtools'>BEDTools</a> package.  This allows one to easily compare/annotate Hydra breakpoint calls to existing annotations in BED format.<br>
<br>
<h2>Hydra breakpoint output file format (.final and .all)</h2>

<table><thead><th> <b>Field Nbr.</b> </th><th> <b>Field Name</b> </th><th> <b>Description</b> </th></thead><tbody>
<tr><td> 1.                </td><td> chrom1            </td><td> Chromosome for end 1 of the breakpoint. </td></tr>
<tr><td> 2.                </td><td> start1            </td><td> Start position for end 1 of the breakpoint. </td></tr>
<tr><td> 3.                </td><td> end1              </td><td> End position for end 1 of the breakpoint. </td></tr>
<tr><td> 4.                </td><td> chrom2            </td><td> Chromosome for end 2 of the breakpoint. </td></tr>
<tr><td> 5.                </td><td> start2            </td><td> Start position for end 2 of the breakpoint. </td></tr>
<tr><td> 6.                </td><td> end2              </td><td> End position for end 2 of the breakpoint. </td></tr>
<tr><td> 7.                </td><td> breakpointId      </td><td> Unique Hydra breakpoint identifier. </td></tr>
<tr><td> 8.                </td><td> numDistinctPairs  </td><td> Number of distinct pairs in breakpoint. </td></tr>
<tr><td> 9.                </td><td> strand1           </td><td> Orientation for the first end of the breakpoint. </td></tr>
<tr><td> 10.               </td><td> strand2           </td><td> Orientation for the second end of the breakpoint. </td></tr>
<tr><td> 11.               </td><td> meanEditDist1     </td><td> Mean edit distance observed in end1 of the breakpoint pairs. </td></tr>
<tr><td> 12.               </td><td> meanEditDist2     </td><td> Mean edit distance observed in end2 of the breakpoint pairs. </td></tr>
<tr><td> 13.               </td><td> meanMappings1     </td><td> Mean number of mappings for end1 of all pairs in the breakpoint. </td></tr>
<tr><td> 14.               </td><td> meanMappings2     </td><td> Mean number of mappings for end2 of all pairs in the breakpoint. </td></tr>
<tr><td> 15.               </td><td> breakpointSize    </td><td> Size of the breakpoint. </td></tr>
<tr><td> 16.               </td><td> numMappings       </td><td> Total number of mappings included in the breakpoint. </td></tr>
<tr><td> 17.               </td><td> allWeightedSupport </td><td> Amount of <i>weighted</i> support from the mappings in the breakpoint. </td></tr>
<tr><td> 18.               </td><td> finalSupport      </td><td> Amount of <i>final</i> support from the mappings in the breakpoint. </td></tr>
<tr><td> 19.               </td><td> finalWeightedSupport </td><td> Amount of <i>final weighted</i> support from the mappings in the breakpoint. </td></tr>
<tr><td> 20.               </td><td> numUniquePairs    </td><td> Number of pairs in the breakpoint that were uniquely mapped to the genome. </td></tr>
<tr><td> 21.               </td><td> numAnchoredPairs  </td><td> Number of pairs in the breakpoint that were mapped to the genome in an "anchored" fashion (i.e. 1xN). </td></tr>
<tr><td> 22.               </td><td> numMultiplyMappedPairs </td><td> Number of pairs in the breakpoint that were multiply mapped to the genome in fashion (i.e. NxN). </td></tr></tbody></table>



<br>
<h2>Hydra breakpoint output detail file format</h2>


<table><thead><th> <b>Field Nbr.</b> </th><th> <b>Field Name</b> </th><th> <b>Description</b> </th></thead><tbody>
<tr><td> 1.                </td><td> chrom1            </td><td> Chromosome for end 1 </td></tr>
<tr><td> 2.                </td><td> start1            </td><td> Start position for end 1 </td></tr>
<tr><td> 3.                </td><td> end1              </td><td> End position for end 1 </td></tr>
<tr><td> 4.                </td><td> chrom2            </td><td> Chromosome for end 1 </td></tr>
<tr><td> 5.                </td><td> start2            </td><td> Start position for end 2 </td></tr>
<tr><td> 6.                </td><td> end1              </td><td> End position for end 2 </td></tr>
<tr><td> 7.                </td><td> name              </td><td> Name of the pair   </td></tr>
<tr><td> 8.                </td><td> mate1End          </td><td> To which mate of the pair do fields 1,2,3,9,11 correspond? (values: 1 or 2) </td></tr>
<tr><td> 9.                </td><td> strand1           </td><td> Orientation for end 1 (+ or -) </td></tr>
<tr><td> 10.               </td><td> strand2           </td><td> Orientation for end 2 (+ or -) </td></tr>
<tr><td> 11.               </td><td> editDist1         </td><td> Alignment edit distance for end 1 (can be extracted from NM tag in SAM) </td></tr>
<tr><td> 12.               </td><td> editDist2         </td><td> Alignment edit distance for end 2 (can be extracted from NM tag in SAM) </td></tr>
<tr><td> 13.               </td><td> numMappings1      </td><td> Number of mappings for end 1 of this pair </td></tr>
<tr><td> 14.               </td><td> numMappings2      </td><td> Number of mappings for end 2 of this pair </td></tr>
<tr><td> 15.               </td><td> mappingType       </td><td> What type of mapping is this? (1=unique, 2=anchored, 3-multiply) </td></tr>
<tr><td> 16.               </td><td> includedInBreakpoint </td><td> Was this pair ultimately included in this breakpoint? </td></tr>
<tr><td> 17.               </td><td> breakpointId      </td><td> Unique Hydra breakpoint identifier. </td></tr>