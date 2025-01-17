# Recurrent Neural Networks
### Introduction
Feedforward and convolutional neural networks provide outputs based on the current inputs. These networks do not consider any inputs or state of the inputs prior to their current states. However, biological neural networks use a lot a recurrent connection, i.e., use the previous inputs in addition to the current inputs. In other words, the biological neural networks "memorize" some of its previous input states and use them later when needed. Recurrent neural networks (RNNs) introduce this "memory" component in the operations performed by the artificial neural networks.

The first attempt to introduce memory in the neural networks was to use time delay neural network (TDNN). In TDNN, the inputs are time-delayed (think of a delay block!) and are fed into the next layers of the network. However, this network can introduce memory of a certain time window, i.e., the memory is limited by the number of the time-delay block. This invention was followed by other simple RNNs, like Elman network or Jordan network. Notably, although these networks paved the paths toward the state-of-the-art developments of RNNs, all these earlier networks suffered from a problem called vanishing gradient.

In mid-nineties, long short-term memory (LSTM) cell was invented to overcome the vanishing gradient problem. The key novel of LSTM is that some signals, also called state variables, are kept fixed by using gates, and reintroduce or not at the appropriate time in the future. In this way, arbitrary time interval can be represented, and temporal dependencies can be captured. Further refinement of this idea was proposed by discovering gradient recurrent units (GRUs).

### Applications of RNNs
* Speech recognition - Examples include Google's Assistant, Apple's Siri, Amazon's Alexa, Nuance's Dragon Solutions, etc.
* Time series prediction - Examples include stock price prediction, traffic patterns prediction, movie selection, etc.
* Natural language processing - Example includes machine translation (Google, salesforce), question answering (Google analytics), chatbots, etc.
* Gesture recognition - Capture gesture by analyzing a sequence of frames, for example, to recognize if a person is waving a hand or swinging it. Qualcomm, EyeSight, Intel, GestureTek, GestSure, Omek are some of the companies who use gesture recognition techniques in many of their products.


### Basic concepts of RNNs
* RNN at time t and t+1
  * At time t+1, state variable s<sub>t</sub> is used as the input to the network. State variable s<sub>t</sub> is the output from the network, specifically, from layer L, at time t. This output goes as the input to the layer L in the next time point. 
* Folded vs. unfolded representations of the RNN
* Single vs. multi-layer RNNs
* Backpropagation through time (BPTT)

# LSTM
  * LSTM with 4 standard gates
    * Forget
    * Learn
    * Remember 
    * Use
  * Peephole LSTM and GRU

# Hyperparameters
  * Optimizer hyperparameters
    * Learning rate - The most important one.
      * Validation error decreasing - learning rate is good.
      * Validation error decreasing slowly - learning rate is low, increase it.
      * Validation error increasing - learning rate is high, decrease it.
      * A good starting point of learning rate is 0.01 but try with other values, e.g., between 0.1 and 0.000001.
    * Learning rate decay - Adaptive learning rate, start with a higher value  and decrease linearly or exponentially after some number of epochs.
    * Mini-batch size
      * Stochastic - batch size is 1.
      * Batch - batch size is equal to the total number of training observations, n.
      * Mini-batch - batch is between 1 and n. Something between 32 and 256 are good choices. Smaller batch size is noisy, larger batch size requires more computational resources.
    * Number of epochs - Early stopping, when validation error does not decrease, say in the last 10 or 20 epochs.
  * Model hyperparameters
    * Number of hidden units - Always tricky to find the best value. Start with a moderate number and apply regularization (L2 or dropout). First hidden layer with unit number larger than the number of inputs typically works better. Very large number of hidden units tend to overfit the model, i.e., validation accuracy is much lower than the training accuracy.
    * Number of hidden layers - 3 is better than 2, more than 3 generally do not help much except in CNN. However, advanced speech recognition might be helpful with 5-7 layers always with LSTM cells (see Soltau et al., 2016).
    * Cell type - RNN, LSTM, GRU. LSTM/GRU are better than RNN. Try with both LSTM and GRU (see Karpathy et al., 2015)
    * Word embedding size - Performance increases with embedding size up to a certain size. 50 to 200 is good but could be 500 or even 1000 (see Lai et al., 2016).

# Embeddings and Word2Vec
  * Word embedding is the collective term for models that learn to map a set of words or phrases in a vocabulary to vectors of numerical values. These vectors are called embeddings. In general, this technique is used to reduce dimensionality of text data. However, this technique can also learn interesting trends and relationships between the words in the vocabulary.
  * Word2Vec model that learns to map words to embeddings that contain semantic meaning. For example, embedding can learn the relationship between present and past tense. Therefore, relationship between walking and walked is like the relationship between swimming and swam, similarly between man and king, and woman and queen.
  * We one-hot-encode the words. So, each word in a 10000-word vocabulary will be represented by a vector length of 10000, of which, only one element is 1 and others are 0s. This approach is computationally inefficient (multiplying a lot of zeros with their weights). 
  * Define a lookup table (LUT). Each row corresponds to a word. Each column represents embedding weights. Let’s consider a 5-word vocabulary and an embedding layer of 3 units. So, the lookup table will have 5 rows and 3 columns as follows:
  <p align="center">
  LUT = [[9,2,7],
         [3,1,5],
         [6,3,6],
         [7,3,8],
         [9,0,4]]
  </p>
  
  * Also, let’s consider that the fourth word’s one-hot-encode vector is: [0, 0, 0, 1, 0], i.e., fourth element is nonzero. Word2Vec looks for the fourth row of LUT and gives as the output to the next layer.
  * There are two architecture for implementing Word2Vec:
    * Continuous bag of words (CBOW): Context words are the inputs and word of interest is the output. 
    * Skip-gram: Word of interest is the input and context words are the outputs. Context words can be two words before and after the word of interest. If `w(t)` is the word of interest, then context words can be `w(t-2)`, `w(t-1)`, `w(t+1)`, and `w(t+2)`.

  
# Sentiment analysis using RNN
* Preprocessing
  * Lowercase and remove punctuation
  * Remove \n

* Encoding words
  * vocab_to_int (sorted by frequency, most frequent words to 1 not 0)
  * int_to_vocab
  * Convert each review to int
  * Convert each label to 1 (+) and 0 (-)

* Standardize
  * Remove zero length reviews
  * Pad or truncate reviews


# Attention
