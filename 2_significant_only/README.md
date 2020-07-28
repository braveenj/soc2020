# Keep only significant data points (pvalue < 0.001)

```
$ more pvalue_sorted.tsv
...
...
ENSGALG00000008934      PIK3CA  0.000992738392868489    -0.3
ENSGALG00000050376              0.000994824004256688    -1.1
ENSGALG00000012082      FAIM    0.000998446529828946    -0.4
ENSGALG00000010999      DICER1  0.00100810371680852     -0.3
ENSGALG00000021193      STARD5  0.00102902792258132     0.4
ENSGALG00000052519              0.00102902792258132     0.5
ENSGALG00000016946      SUGT1   0.00103327849979457     -0.3
```

ENSGALG00000012082 is the last line we want to keep. Let's get rid of the rest. Let's find out the line number of this line

```
$ cat -n pvalue_sorted.tsv |  more
  2203  ENSGALG00000007519      ACBD5   0.000991466118048178    -0.4
  2204  ENSGALG00000008934      PIK3CA  0.000992738392868489    -0.3
  2205  ENSGALG00000050376              0.000994824004256688    -1.1
  2206  ENSGALG00000012082      FAIM    0.000998446529828946    -0.4
  2207  ENSGALG00000010999      DICER1  0.00100810371680852     -0.3
  2208  ENSGALG00000021193      STARD5  0.00102902792258132     0.4
  2209  ENSGALG00000052519              0.00102902792258132     0.5
  2210  ENSGALG00000016946      SUGT1   0.00103327849979457     -0.3
  2211  ENSGALG00000009915      EML4    0.00104024036341589     0.6
```
Line 2206!!

Or we could use grep 
```
$ grep -n -e "ENSGALG00000012082\tFAIM\t0.000998446529828946\t-0.4" pvalue_sorted.tsv
2206:ENSGALG00000012082	FAIM	0.000998446529828946	-0.4
```
Line 2206!!

Now let's keep only the first 2,206 lines.
```
$ head -n 2206 pvalue_sorted.tsv  | tail
ENSGALG00000016687	P2RY8	0.000951053681334641	0.3
ENSGALG00000003807	GPN3	0.000951921762249093	-0.3
ENSGALG00000002024	COMT	0.000956279026557719	-0.3
ENSGALG00000041025	FOXJ1	0.00095977359077977	0.8
ENSGALG00000005823	HDLBP	0.000981557512422558	-0.4
ENSGALG00000005769	WEE1	0.000990275329888021	-0.3
ENSGALG00000007519	ACBD5	0.000991466118048178	-0.4
ENSGALG00000008934	PIK3CA	0.000992738392868489	-0.3
ENSGALG00000050376		0.000994824004256688	-1.1
ENSGALG00000012082	FAIM	0.000998446529828946	-0.4
```
Great, the last line is the one we want. Let's create a new file.

```
$ head -n 2206 pvalue_sorted.tsv > pvalue_sorted_significant_only.tsv
```
Check it to make sure it looks right.

Check the top:
```
$ head pvalue_sorted_significant_only.tsv
Gene ID	Gene Name	g1_g2.p-value	g1_g2.log2foldchange
ENSGALG00000002286	H3F3B	0	-7
ENSGALG00000020078	H3F3C	0	-8
ENSGALG00000027353	C10orf71	1.29880461971355e-212	-3.7
ENSGALG00000030407	IL6R	3.8485270968074e-183	2.4
ENSGALG00000037669	FKBP9	3.58400895215323e-177	-3.6
ENSGALG00000031786	BLVRA	5.34950331436035e-177	2.2
ENSGALG00000023279	EPN3	3.12236315199182e-170	-2.9
ENSGALG00000011551	JCHAIN	6.47982639371527e-144	-1.7
ENSGALG00000002911	MOXD1	1.29318024850042e-142	-2.4
```

Check the bottom of the file:
```
$ tail pvalue_sorted_significant_only.tsv
ENSGALG00000016687	P2RY8	0.000951053681334641	0.3
ENSGALG00000003807	GPN3	0.000951921762249093	-0.3
ENSGALG00000002024	COMT	0.000956279026557719	-0.3
ENSGALG00000041025	FOXJ1	0.00095977359077977	0.8
ENSGALG00000005823	HDLBP	0.000981557512422558	-0.4
ENSGALG00000005769	WEE1	0.000990275329888021	-0.3
ENSGALG00000007519	ACBD5	0.000991466118048178	-0.4
ENSGALG00000008934	PIK3CA	0.000992738392868489	-0.3
ENSGALG00000050376		0.000994824004256688	-1.1
ENSGALG00000012082	FAIM	0.000998446529828946	-0.4

```

Looks great! We have only our signficant data points sorted by pvalue.