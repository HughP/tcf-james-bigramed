# Mephaa keyboard analysis

**Language**: [tcf]

**Text source**: Mark Weathers

**Text content**: Epistle of James

**Keyboard layout history**: The Spanish Windows ANSI keyboard layout was co-opted with redundant Spanish characters replaced by Meꞌphaa characters. This was done in conjunction with replacing the glyphs in a special font, so that no keyboard setting needed to be changed on the computer. Simply type as normal on a Spanish keyboard and use the Mephaa file. This results in a keyboard layout that has been used for some years by some members of the Meꞌphaa community. The layout for this keyboard is presented below and is available as JSON from [here](http://www.keyboard-layout-editor.com/#/gists/12b42baca7030bfabea5181833232e2b).

**Meꞌphaa keyboard layout images**:

---

![Base State](Images/Mephaa-state-0.png)

_Base State_

---

![Base State](Images/Mephaa-state-shift.png)

_Shift State_

---

![Base State](Images/Mephaa-state-alt.png)

_Alt State_

---

![Required Usage](Images/tcf-heatmap-with-full-text.png)

_Mephaa Required Usage_

---

**Text processing steps**:

1. Text received, as `JAS_TCF.txt`
2. Moved characters from hacked font code points to proper Unicode values, using [`Teckit`](https://github.com/silnrsi/teckit). `me'phaa.map` & `me'phaa.tec`
3. Scripture texts has a very formal typesetting process. Things like paragraph, book, chapter, and verse markers: these are indicated by a reverse solidus `\`. All of these are removed (by hand).
4. Replaced all characters in the Meꞌphaa text with their corresponding values as if they were English characters typed on a QWERTY keyboard. (Done by hand via search and replace.) resulting file: `tcf-on-QWERTY.txt`
 This allows for [`Typing`](https://github.com/michaeldickens/Typing) to process the characters (really in the mental model of typing it is processing keypresses not characters). [`Typing`](https://github.com/michaeldickens/Typing) only processes characters as if they are single byte, so no two or three byte characters work with the program. However, this means that if a language corpus is converted from their orthographical representation it can be re-rendered as a keypress representation. This keypress representation can just so happen to have QWERTY codepoints - the result is not English, rather some language as goblety gook. Another way to think about this would be to use ISO 9995 names of keys.
5. `tcf-on-QWERTY-UCC.txt` is a quick check to show that all characters in the file are in the single byte range.
6. [`Typing`](https://github.com/michaeldickens/Typing) requires a list of character bigrams and a list of character counts.
 The default method is to use an application by Michael Dickens called [`Frequency`](https://github.com/michaeldickens/Frequency). - Hugh has had some difficulty in getting that to compile (and it was not guaranteed to work with multi-byte characters which was another requirement). So in lieu of using that Hugh started down the path of step _Seven_.
 [`Typing`](https://github.com/michaeldickens/Typing) assumes that there is a one to one correspondence between each single byte character and each keystroke. Processes in step three ensure that all all multi-byte characters are converted to single byte characters and their corresponding positions. This can allow [`Typing`](https://github.com/michaeldickens/Typing) to give us a fitness value (by running the tests against the existing QWERTY setting), it can also allow [`Typing`](https://github.com/michaeldickens/Typing) to make a projection about how to organize a keyboard layout based on [`Typing`](https://github.com/michaeldickens/Typing)'s simulated annealing algorithm.
7. To create bigrams and character count the following scripts were used:

  ```
./bigrams.py tcf-on-QWERTY.txt > allDigrams.txt
 ```
 Then to get the character counts.
  ```
 UnicodeCCount.pl -n tcf-on-QWERTY.txt | cut  -f 2,3 | tr "\t" " " > all
 Characters.txt && sed -i '1d' allCharacters.txt
 ```

 Then the character for new line had to be added to the top line as `\n`.

8. Eventually just KLA was used with the text from `tcf-on-QWERTY.txt`. This text was then re-encoded back to Meꞌphaa and an image created via [KLE](http://www.keyboard-layout-editor.com/#/gists/6db30572ab8b8ee09eadedfb57f99eea). The analysis of KLA is presented below.
9.  Assumed total keypress count for Meꞌphaa was computed as follows:

 ```
UnicodeCCount.pl tcf-on-QWERTY.txt | tail -n +2 | cut -f 3 | paste -sd+ | bc
 ```
   This results in a total of 22235 keypresses. It is assumed that becase we are counting the text after it has been converted to QWERTY that we are no longer counting characters, but we are counting what they represent, keypresses.
   By using the following command

 ```
   UnicodeCCount.pl mephaa3-unicode.txt  | tail -n +2 | cut -f 3 | paste -sd+ | bc
```
   we see that there are only 21294 characters (NFC) in the Meꞌphaa text. If then take out the 1879 units of U+0331 (it is a combining character) then we get the total number of "reading characters" (like letters, but without evoking the idea of functional units - I'm counting diacritics with their bases - and I am not counting `ñ` as a separable character). For a grand total of 19415 letters. 22235 key presses to produce 19415 letters. A ratio of 1.145 keys per letter. 4176 total diacritics, for a diacritic (to character) density 21.51%. Or of one diacritic per every 4.2428 letters (not including `ñ`). If we include `ñ` the total increases to 4296 and the density to 22.12% or one diacritic per every 4.51 "letters".


<!-- 7. To create bigrams the service at the following website was used: https://www.dcode.fr/bigrams. The following settings were also used:
   *  ALL CHARACTERS (INCLUDING PUNCTUATION AND SYMBOLS)
   * STANDARDIZATION OF LETTERS (IGNORE UPPER-LOWER CASE AND DIACRITICS) [un-checked]
   * Analyze BY SLIDING (ABCDEF => AB,BC,CD,DE,EF)
   * KEEP WORDS BORDERS (ABC_DE ≠ ABCDE) [checked]
   * COUNT APPEARANCES


  ![Bigram Options](Images/Bigram-counting.png)


  The website produces a down-loadable `.csv` file `tcf-on-QWERTY-bigram-count-ori.csv`. Some editing of this CSV file is necessary to convert it into the same format of bigram file that [`Typing`](https://github.com/michaeldickens/Typing) expects (`\n` for new line, `\\` for `\`, `\t` for TAB, and only a space between the character column and the count column). -->

## Keyboard Layout Analysis
**Statistical analysis of exiting and optimized Meꞌphaa keyboard using Keyboard Layout Analyzer (KLA)**

Using the text transformation methods outlined above the following keyboard statistics become available when using [KLA](http://patorjk.com/keyboard-layout-analyzer/#/main).

KLA also suggests an "optimized" keyboard, and is the reference keyboard layout in the following graphs. This is contrasted with the existing Meꞌphaa keyboard which is shown above. In the diagrams the KLA optimized keyboard is referenced as _Personalized_ while the existing Meꞌphaa layout is labeled _QWERTY_. It should be noted that the KLA optimization engine acknowledges that it is not a very aggressive optimization.  One place or issue that Hugh notices is where further optimization could be considered is that both tone marks could be moved to the right hand so that a better cadence can be achieved. As the tone marks currently are situated a high level of outward rolls exist.  

![proposed optimized keyboard](Images/tcf-kla-keyboard-layout.png)

The distance that the typists' fingers will need to travel is greater for the exiting Meꞌphaa layout.

![Mephaa distance traveled](Images/tcf-distance.png)

As the previous heat map for the existing Meꞌphaa keyboard shows, the frequently used keys are on the periphery of the typing area, significantly overloading the weaker fingers. In actual typing of Meꞌphaa, Hugh has observed this to contribute to hunt-and-peck style typing.

However, the severity of how much more work to type it is is not revealed until we compare the text input task on the Meꞌphaa keyboard with other keyboards. There are two ways to compare:
  1. Compare within the Meꞌphaa language to other keyboard layouts supporting mephaa
  2. Compare to other language based options that Meꞌphaa speakers might use to communicate the same message. The following graphs illustrate both points.

![Meꞌphaa distance traveled](Images/tcf-finger-load.png)

![Meꞌphaa distance traveled charts](Charts/tcf-eng-spa-total_traveled_distance.svg?sanitize=true)

<!-- tcf-eng-spa-finger-usage-chart.svg -->

In terms of work load percentages we can see where on the hand the two keyboards are "balancing" the workload.

![Meꞌphaa distance traveled](Images/tcf-percentage-load.png)

Finally in terms of row usage we can see where the high frequency targets are.

![Meꞌphaa distance traveled](Images/tcf-row-usage.png)

---

 ![Keyboard ISO 9995 Key numbers](Images/Keyboard-Key-IDs.png)

_Keyboard ISO 9995 Key numbers on an ANSI QWERTY keyboard_

---

 ![Handedness on keyboards](Images/Keyboard-Handedness.png)

_Keyboard Handedness shown with ISO 9995 Key numbers on an ANSI QWERTY keyboard_

---

<!-- Link to keyboard file:  http://www.keyboard-layout-editor.com/#/gists/12b42baca7030bfabea5181833232e2b -->


<!-- comparing with Spanish:

```
join -a 1 -1 1 -2 1 -t $'\t' -o 1.1,1.2,2.3,1.3,1.4  mephaatable.txt spanishtable.txt
```

U+000A	 		173	LINE FEED
U+0020	 	2139	3047	SPACE
U+0021	!	4	2	EXCLAMATION MARK
U+0028	(		1	LEFT PARENTHESIS
U+0029	)		1	RIGHT PARENTHESIS
U+002C	,	183	216	COMMA
U+002E	.	96	177	FULL STOP
U+0030	0		8	DIGIT ZERO
U+0031	1		61	DIGIT ONE
U+0032	2		30	DIGIT TWO
U+0033	3		13	DIGIT THREE
U+0034	4		13	DIGIT FOUR
U+0035	5		13	DIGIT FIVE
U+0036	6		12	DIGIT SIX
U+0037	7		11	DIGIT SEVEN
U+0038	8		9	DIGIT EIGHT
U+0039	9		8	DIGIT NINE
U+003A	:	17	20	COLON
U+003C	<		32	LESS-THAN SIGN
U+003E	>		32	GREATER-THAN SIGN
U+0041	A	15	121	LATIN CAPITAL LETTER A
U+0044	D	21	2	LATIN CAPITAL LETTER D
U+0045	E	15	1	LATIN CAPITAL LETTER E
U+0047	G		21	LATIN CAPITAL LETTER G
U+0049	I	2	30	LATIN CAPITAL LETTER I
U+004A	J	4	25	LATIN CAPITAL LETTER J
U+004B	K		6	LATIN CAPITAL LETTER K
U+004D	M	4	9	LATIN CAPITAL LETTER M
U+004E	N	11	31	LATIN CAPITAL LETTER N
U+004F	O	1	2	LATIN CAPITAL LETTER O
U+0050	P	21	6	LATIN CAPITAL LETTER P
U+0052	R	1	11	LATIN CAPITAL LETTER R
U+0053	S	27	5	LATIN CAPITAL LETTER S
U+0054	T	8	16	LATIN CAPITAL LETTER T
U+0058	X		45	LATIN CAPITAL LETTER X
U+005B	[		1	LEFT SQUARE BRACKET
U+005C	\		173	REVERSE SOLIDUS
U+005D	]		1	RIGHT SQUARE BRACKET
U+0061	a	1136	3028	LATIN SMALL LETTER A
U+0062	b	159	292	LATIN SMALL LETTER B
U+0063	c	317	11	LATIN SMALL LETTER C
U+0064	d	456	225	LATIN SMALL LETTER D
U+0065	e	1251	350	LATIN SMALL LETTER E
U+0066	f	76	5	LATIN SMALL LETTER F
U+0067	g	96	455	LATIN SMALL LETTER G
U+0068	h	115	315	LATIN SMALL LETTER H
U+0069	i	573	1578	LATIN SMALL LETTER I
U+006A	j	41	479	LATIN SMALL LETTER J
U+006B	k		302	LATIN SMALL LETTER K
U+006C	l	466	342	LATIN SMALL LETTER L
U+006D	m	283	847	LATIN SMALL LETTER M
U+006E	n	580	1769	LATIN SMALL LETTER N
U+006F	o	976	641	LATIN SMALL LETTER O
U+0070	p	221	82	LATIN SMALL LETTER P
U+0071	q	115	2	LATIN SMALL LETTER Q
U+0072	r	658	483	LATIN SMALL LETTER R
U+0073	s	800	275	LATIN SMALL LETTER S
U+0074	t	369	374	LATIN SMALL LETTER T
U+0075	u	434	1184	LATIN SMALL LETTER U
U+0076	v	113	108	LATIN SMALL LETTER V
U+0077	w		173	LATIN SMALL LETTER W
U+0078	x	3	357	LATIN SMALL LETTER X
U+0079	y	127	128	LATIN SMALL LETTER Y
U+00A1	¡		2	INVERTED EXCLAMATION MARK
U+0301			2297	COMBINING ACUTE ACCENT
U+0303			120	COMBINING TILDE
U+0331			1879	COMBINING MACRON BELOW
U+A78B	Ꞌ		1	LATIN CAPITAL LETTER SALTILLO
U+A78C	ꞌ		1221	LATIN SMALL LETTER SALTILLO
U+FEFF			1	ZERO WIDTH NO-BREAK SPACE

-->
