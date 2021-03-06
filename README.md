# word.tagging
A word tagging project created during my master thesis with the AUEB NLP Group. In this project various machine learning are being tested and compared in order to check their accuracy when performing part of speech tagging on greek words 
. Nevertheless the code was developed in such a way, that it allows 1-of-a-K classification of words regardless of language. This means that you can use this software for other tasks of this type, such as Named Entity Recognition.

More info regarding the master thesis and the NLP Group of AUEB can be found:
http://nlp.cs.aueb.gr/

An addition to the word tagging project is a tokenizer implemented with song tokenization in mind.

## Prerequisites
 1.   Unix or Windows Computer and admin rights to it
 2.   Java 8+
 3.   >8GB memory else you have to tweak the code to be stream based, which I don’t recommend, since it may be much much more slower
 4.   Nvidia GPU with 4+GB RAM and a lot of Cuda cores if you are planning to use large sets and deeplearning4j

## Required Knowledge
 1.   Very Good knowledge of Java (8)
 2.   Average knowledge of Unix or Windows bash
 3.   Basic Knowledge of maven and packaging
 4.   If you plan to mess around with native code, good knowledge of C, C++ and or Cuda.

## How to install
 1.   Install Maven on your system
 2.   Install DeepLearning4j (at time of writing Snapshot-3.9). 
 3.   Have some training data in the CONLL format.
 4.   Have some test data in the CONLL format.
 5.   If you want to classify new data just use them in CONLL format
 6.   SVMs work only for Windows, If you need them for Unix either wrap Stanford’s SVMFactory Class like I did or maybe use another package.
 7.   To use the tokenizer on a song, the song lyrics must be stored in a directory. The lyrics must agree with the common lyric format (each lyric in a line, empty line between verses, repetition marks the lyrics to be repeated etc.)

## How to use programmatically
 1.   Create a new Class with a main
 2.   Create a new ExperiementSetup.Builder and build it after setting the appropriate variables for the experiement.
 3.   Use the Javadoc
 4.   The tokenizer can be used by itself through the main method it includes or be used by another class. In the first case, just call       it's main method (after setting the file path). In the second case, create a SongTokenizer object, read the file into a String           with the SongHelper class and call the tokenize method (from the SongTokenizer class) with the aforementioned String as an input.

## System Inputs
 1.  All the possible Categories, provided in a file, separated with new line (\n). Example:  
  *Category1*  
  *Category2*  
  *Category3*  
  *…*  
  *eof (end of file)*
 2.   Word-Category files as wordsets (trainset, testset etc). Each word-category pair is in the same line separated with any preferred delimeter (space and tab are recommended). You have to set the delimeter in code else the space is used. Each Pair is separated via newline \n. Example:  
  *word1 Category1*  
  *word2 Category3*  
  *word3 Category1*  
  *…*  
  *eof*  
 3.   Embedding file. In this file we have a word-double valued vector Pair. The token is always first and then follow the double values of the vector. Default Delimeter is space:” ”. Each pair is separated by new line. Example:  
  *word1 1.0 2.0 3.0 4.0 5.0*  
  *word2 2.0 3.0 4.0 5.0 6.0*  
  *…*  
  *eof*  
 4.   Gazzete’s or dictionaries with words that contain Category-Word spairs can be used to classify directly known words/tokens. Pairs are separated with each other by the use of new line. In this case the word-category separator is always space. Example:  
  *word1 cat1*  
  *word2 cat2*  
  *…*  
  *eof*  
 5.   Special Categories and words. The Pair: 
     *%newarticle% null*
  is used  to separate word sequences with each other. This means that Sequence classifiers (such as CRF and LSTM) as well as the Feature Builder will stop looking before and after they encounter that pair. SO AVOID USING null AS A CATEGORY and %newarticle% as a word/token. 

 6.   For the tokenizer, the only input is the path to the file with the song lyrics(has to be set either in the SongTokenizer.main or when invoking the SongHelper.readSongFile, which stores the file content into a String).

## Evaluation & Metatagging
   After a classifier provides a classified wordset, it can be evaluated against the original. This happens through the evaluation class. Also a metatagger with programmed rules is provided. See the code and the Javadoc for more information, since those 2 classes are depended on what you plan to do and you should change/reimplement them if needed. 

## DeepNets
   For evaluation several deep learning packages have been used. The LSTM in the Thesis text belongs to Wang Ling. I have made some changes in its code but due to incompatibility with current dl4j libraries I ve decided to keep as a separate project. You can ask me for the code at asikis.thomas@gmail.com.

   For the implementation of other deep nets, currently the computation graph of dl4j is used. See more about it here: http://deeplearning4j.org/compgraph

## SVMs
   Have only been implemented for Windows 64 bit by wrapping the Windows Factory of Stanford CoreNLP and providing the appropriate binaries. For other distributions you might have to do it yourself.

## Data
   For now I am providing some small data files in Greek, just for testing purposes. Since the size of the experimental data is more than 500MB zipped, email me on asikis.thomas@gmail.com. and I will provide you with a download link.
   
## Tokenizer
   The tokenizer, when used as a standalone tool via its main method, will print out the tokenized song. If it's used by another class,    the tokenize method returns the List\<String\> containing the song which can be used as necessary.
   
   The tokens are:
   
   - |\<new verse\>|, which marks a new verse
   - |\<new lyric\>|, which marks a new lyric
   - |\<repetition start\>|, which marks the start of a repetition*
   - |\<repetition end\>|, which marks the end of a repetition end*
   - |\<word\>|, which marks a new word(in the context of song tokenization, this token is unnecessary and therefore discarded from the        List\<String\>)
   
   *when the two tokens are placed one after another in the list, it means that only the following lyric is repeated.


