# NeuralNetwork_Dependency-Parser


         Natural Laguage Processing Assignment 3
 
==================================================================
Implementation :

apply () Function: First I had to implement the apply function where the task was to employ the arc-standard system as defined in Nivre’s paper. I had to implement this function in ParsingSystem.py.
As per the arc-standard system, there are three basic type of transitions, namely left reduce, right reduce and shift which I had to implement. There were some predefined functions already in Configuration file, so I didn’t have to write those separately. I had to do some function calls for this part and finally return the result. The implementation for the three steps has been explained below:

Shift: Here I checked if my transition started with character (“S”). If it did I called the function shift to append the token onto the stack.
Left Reduce: I extracted the two topmost tokens (say wi, wj) and stored it in a variable label. Then I checked if the transition started with character “L”. If it did I pushed the elements onto the stack by adding an arc from wj->wi and reduced to wj by calling function removeSecondTopStack()
Right Reduce: I extracted the two topmost tokens (say wi,wj) and stored it in a variable label. Then I checked if the transition started with character “R”. If it did I pushed the elements onto the stack by adding an arc from wi->wj and reduced to wi by calling function removeTopStack.()

getFeatures() Function : I had to implement the get feature function where the task was to extract the words ,POS tags and labels from the stack as defined in Manning and Chen’s paper. I had to implement this function in DependencyParser.py.
1.Here first I defined three list variables sw, st and sl for extracting and storing the words, POS tags and labels. I ran a loop to extract the top three words and tags from stack and buffer and stored it into the corresponding list.
2. Again I ran a loop to extract the first and second leftmost and rightmost children of the top two words on the stack and did the same thing for POS tags and labels.
3. Finally the leftmost of leftmost and rightmost of rightmost children of the top two words on the stack were extracted. Same was done for POS tags and label.

For all the above extraction, the getwordID function was called to extract the word id which had been defined in DependencyParser.py. Also, I called functions getLeftChild, getWord from the Configuration file to get the words, tag and labels from the stack. Finally, the words, pos tags and labels were all concatenated in a single list “final” of length 48. 

forward_pass() Function : Next I had to implement the neural network architecture. The input layer here was the word/pos/label embedding which we calculated in the feature function. The embeddings are then passed to the hidden layer where the cube activation function is implemented. Firstly, the embedding is multiplied with the weights. Here we took the transpose of embedding for the matmul operation to execute and then added the result of this multiplication with the bias and found out the cube linearity of the result. Then in the softmax layer I multiplied the result with weights output and returned this to the build_graph() function to calculate the loss.

build_graph() Function : The network and its variables had to be defined in build_graph(). The placeholders for train_inputs, train_labels and test_inputs were defined with appropriate shapes. The variables for the weights (both input and output) and biases were defined with weights being initialized as a tensorflow variable having truncated normal distribution with a standard deviation of 0.1. The biases were initialized to zero values. In both the train and test cases, the inputs were used to look up the embeddings which were then fed with the weights and biases to the forward pass function. The predictions are calculated from the forward pass function and then the loss is calculated (only for train data) by implementing the sparse cross entropy loss function of tensorflow, along with the result of argmax on the train_labels. The l2 losses are calculated for all the parameters like weights, biases and embed and multiplied with a lamda value. The cross-entropy loss and the l2 loss are then added to give us the final loss.
