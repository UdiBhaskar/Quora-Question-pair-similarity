# Quora Question Pair Similarity

Over 100 million people visit Quora every month, so it's no surprise that many people ask similarly worded questions. Multiple questions with the same intent can cause seekers to spend more time finding the best answer to their question, and make writers feel they need to answer multiple versions of the same question. Quora values canonical questions because they provide a better experience to active seekers and writers, and offer more value to both of these groups in the long term.  so main aim of project is that predicting whether pair of questions are similar or not. This could be useful to instantly provide answers to questions that have already been answered.
### Problem Statement :
Identify which questions asked on Quora are duplicates of questions that have already been asked.

### Real world/Business Objectives and Constraints :
1. The cost of a mis-classification can be very high.
2. You would want a probability of a pair of questions to be duplicates so that you can choose any threshold of choice.
3. No strict latency concerns.
4. Interpretability is partially important.

### Data Overview:
Train.csv contains 5 columns : qid1, qid2, question1, question2, is_duplicate

i derived some additional features from questions like no of common words, word share and some distances between questions with the help of word vectors. will discuss those below.
### Some Analysis:
- ##### Distribution of data points among output classes
   ![No of Datapoints per Class](https://github.com/UdiBhaskar/Quora-Question-pair-similarity/blob/master/Images/output_30_1.png "No of Datapoints per Class") 
- ##### Number of unique questions
   ![Number of unique questions](https://github.com/UdiBhaskar/Quora-Question-pair-similarity/blob/master/Images/output_35_0.png "Number of unique questions") 
- ##### Number of occurrences of each question
   ![Number of occurrences of each question](https://github.com/UdiBhaskar/Quora-Question-pair-similarity/blob/master/Images/output_39_1.png "Number of occurrences of each question")
- ##### There is no duplicate pairs. Have 2 Null values, which are filled with space.
- ##### Wordcloud for similar questions
   ![Wordcloud for similar questions](https://github.com/UdiBhaskar/Quora-Question-pair-similarity/blob/master/Images/output_71_1.png "Wordcloud for similar questions")
- ##### Wordcloud for dissimilar questions
   ![Wordcloud for dissimilar questions](https://github.com/UdiBhaskar/Quora-Question-pair-similarity/blob/master/Images/output_73_1.png "Wordcloud for similar questions")
### Feature Extraction:
- ##### Extracted some features before cleaning of data as below. i will call as Basic Features.
  - <b>freq_qid1</b> = Frequency of qid1's
  - <b>freq_qid2</b> = Frequency of qid2's
  - <b>q1len</b> = Length of q1
  - <b>q2len</b> = Length of q2
  - <b>q1_n_words</b> = Number of words in Question 1
  - <b>q2_n_words</b> = Number of words in Question 2
  - <b>word_Common</b> = (Number of common unique words in Question 1 and Question 2)
  - <b>word_Total</b> =(Total num of words in Question 1 + Total num of words in Question 2)
  - <b>word_share</b> = (word_common)/(word_Total)
  - <b>freq_q1+freq_q2</b> = sum total of frequency of qid1 and qid2
  - <b>freq_q1-freq_q2</b> = absolute difference of frequency of qid1 and qid2
- ##### Did some preprocessing of texts and extracted some other features. called as Advanced features. i am giving some definitions which are used below. `Token`- You get a token by splitting sentence by space  ,  `Stop_Word` - stop words as per NLTK, `Word `-A token that is not a stop_word.
  - <b>cwc_min</b> = common_word_count / (min(len(q1_words), len(q2_words)) 
  - <b>cwc_max</b> = common_word_count / (max(len(q1_words), len(q2_words)) 
  - <b>csc_min</b> = common_stop_count / (min(len(q1_stops), len(q2_stops)) 
  - <b>csc_max</b> = common_stop_count / (max(len(q1_stops), len(q2_stops)) 
  - <b>ctc_min</b> = common_token_count / (min(len(q1_tokens), len(q2_tokens)) 
  - <b>ctc_max</b> = common_token_count / (max(len(q1_tokens), len(q2_tokens)) 
  - <b>last_word_eq</b> = Check if Last word of both questions is equal or not (int(q1_tokens[-1] == q2_tokens[-1]))
  - <b>first_word_eq</b> = Check if First word of both questions is equal or not (int(q1_tokens[0] == q2_tokens[0]) )
  - <b>abs_len_diff</b> = abs(len(q1_tokens) - len(q2_tokens))
  - <b>mean_len</b> = (len(q1_tokens) + len(q2_tokens))/2
  - <b>fuzz_ratio</b> = How much percentage these two strings are similar, measured with edit distance.
  - <b>fuzz_partial_ratio</b> = if two strings are of noticeably different lengths, we are getting the score of the best matching lowest length substring.
  - <b>token_sort_ratio</b> = sorting the tokens in string and then scoring fuzz_ratio.
  - <b>longest_substr_ratio</b> = len(longest common substring) / (min(len(q1_tokens), len(q2_tokens))
- ##### Extracted Tf-Idf features for this combained question1 and question2 and got 1,2,3 gram features with Train data. Transformed test data into same vector space. 
- ##### Got [Word Movers Distance](http://proceedings.mlr.press/v37/kusnerb15.pdf) with google pretrained word vectors. 
- ##### From Goggle Pretrained word vectors got average word vector for question1 and question2. With this avg word vector got below distances. 
  - <b>Cosine distance</b>
  - <b>Cityblock distance</b>
  - <b>Canberra distance</b>
  - <b>Euclidean distance</b>
  - <b>Minkowski distance</b>
  
  
