This is a personal repository of tools for interfacing with treebanks as a part of the Universal Dependencies (UD) project.

## Consistency

A problem in the UD treebanks is ensuring they are self consistent. This is looked at in Boyd et al (2013) and is implemented in this repository. The first script consistency.py takes the name of a conll file and analyzes its consistency. The output is as follows.

```
lemma1, lemma2
    R, nsubj at 12345
    L, aux at 472
    L, mwe at 65378

lemma3, lemma4
    L, dobj at 76
    R, name at 1
```

The unindented lines consist of a lemma pair which are the two lemmas that make up a variation nuclei. This variation nuclei is listed because it may be that the given occurences in the treebank are inconsistent. For reference, right headed means that the head of the relation is to the right of the child and left headed means the head of the relation is to the left of the child. For example lemma1 and lemma2 are annotated in several different ways. It is right headed, with a nsubj relation between the lemmas at line 12345. Similarly, it is annotated as left headed with an aux relation on line 472. These two occurences appear to be inconsistent to the program and should be checked.

Run the consistency script as follows:

```
./consistency.py corpus.conllu > output.txt
```

Checking and annotating the occurrences are done in the following manner.

```
lemma1, lemma2
    R, nsubj at 12345 y
    L, aux at 472 n
    L, mwe at 65378 n

lemma3, lemma4
    L, dobj at 76 y
    R, name at 1 n
```

A `y` means that the occurence was correct in the original corpus, and a `n` means that the occurence was not correct in the original corpus. After annotating the entire file in this manner you can now run it with analyze.py. Note that are are some formatting restrictions on the file to be analyzed. Each `y` or `n` must be the last character in the line except for whitespace.

analyze.py will output the accuracy of the consistency.py script based on the annotations in the consistency.py output. It also provides the total amount of annotated occurences.

Run the analyze script as follows:

```
./analyze.py output.txt
```

### Transfer

To transfer annotations between files the transfer.py script was created. This is especially useful when one consistency output has already been annotated and there is another one consistency output that used a more stringent heuristic that is a subset of the first.

Run the transfer script as follows:

```
./transfer.py output1.txt output2.txt
```

### Compare

Another useful tool in the consistency workflow is the compare script. This is related to the transfer script in terms of the files used. It is assumed that there is one annotated consistency output file and another consistency output file that is a subset of the first. It does not matter if the second output file is annotated or not.
