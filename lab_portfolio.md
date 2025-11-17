# Feedback on lab 1

## Teacher feedback
`Task 1.01:` Your solution is essentially identical to the reference solution.

`Task 1.02:` The check for `len(ids) > 0` is unnecessary to execute in each iteration of the `while` loop, since you are never changing `ids`. I also did not understand the check `len(ids) > 2` after the `while` loop. Is that check supposed to handle some edge case? I believe your code could be simplified:

def replace(ids: list[int], pair: Pair, new_id: int) -> list[int]:
    result = []
    i = 0
    while i < len(ids):
        if ids[i] == pair[0] and i < len(ids) - 1 and ids[i + 1] == pair[1]:
            result.append(new_id)
            i += 2
        else:
            result.append(ids[i])
            i += 1
    return result

`Task 1.03:` Your explanation of encoding and decoding shows that you understand the BPE algorithm.

`Task 1.04:` Your solution is essentially identical to the reference solution. Having said that, have you compared your saved tokeniser to the provided one using the `diff` tool? It appears you created yours with a vocabulary size of 1256, whereas the reference tokenisers have a vocabulary size of 1024 (256 single-byte tokens + 768 merge rules). This may explain the observation you added to your comment:

I tested the training fucntion with 2 files the swedish and english versions and got identical resutl, however, I believe the provided tokenizer is not complete since I am  getting more lines than the one provided.

You are getting more lines because you are using a larger vocabulary.

`Task 1.05:` Your results are expected, but I think you can sharpen your reasoning about why they happen. In particular, did you notice that all words correspond to single tokens in the ChatGPT tokeniser? (Use the Tiktokenizer website to find out.) One hypothesis, then, is that the model cannot “see through” the token and access its individual letters – which it would need to reverse the word successfully.

You did not provide other prompts that illustrate problems related to tokenisation.

`Task 1.06:` You got about halfway through this task but did not address the final and most interesting part: “Interpreting the expected number of Unicode characters as a measure of representation efficiency, what do your results tell you about the efficiency of a language model primarily trained on English data when it is used to process non-English data? Why are these findings relevant?” Please think about this ahead of the oral exam.

You asked:

- In task `1.06` What do we mean with testing the tokeniser?

Here, “testing” means “applying the tokeniser to see what results you get”.

`Task 1.07:` Your answer is not correct:

It is a way for representing the words of a text by writing the number of occurences of each word. Each embedding layer represent a word to embed, so `num_embeddings` is basically the number of words to embed.

An embedding does not represent the number of occurrences of each word. Also, each embedding layer represents a mapping for the complete vocabulary, not only a single word.

You are missing other crucial pieces in your explanation. The input `x` to the bag-of-words classifier is a text represented as a vector of token IDs. The first step is to look up the embedding vectors for the token IDs and then calculate their element-wise mean. All token IDs are mapped using the same embedding, which is shown in the diagram with dotted lines linking the three embedding layers – thus, there is actually only one embedding layer. The keyword argument `dim=-2` ensures that the mean is calculated across the columns of all vectors, rather than within each single vector. The mean vector is passed through the final linear layer. Unlike what is shown in the diagram, there is no softmax at the end – the classifier outputs unnormalised logits instead.

`Task 1.08:` This one looks fine to me.

`Task 1.09:` Looks good, save for the fact that you did not implement the factorisation with keyword arguments that was requested in Step 2. Also, you do not use the argument `label`, meaning that you will always train on one task (label=0).

`Task 1.10:` You do not answer the question of which task is harder. Since you always train on the sentiment task (see my previous comment), you will get very similar loss values for both calls. Please fix your training loop and redo this task ahead of the oral exam.

`Task 1.11:` You will have to redo this analysis after fixing your code.

`Task 1.12:` You do not comment your results. Did you notice an effect? In my experiments, using the Kaiming initialisation reduces the loss of both models: With the same random seed as before, the loss for the topic model is now 0.1017 for the topic classification model (a 54% reduction) and 0.3798 for the sentiment classification model (a 34% reduction).

## My improvements
`1.02` the code is simplified as suggested.
`1.04` The vocabulary size is changed to the suggested one 1024
`1.06` fixed as suggested and added my interpretation regarding the number of unicode characters.

`1.07` fixed based on the suggested answer

`1.09` I refactored the train function to use keyword arguments (filename, n_vocab, embedding_dim, learning_rate, batch_size, num_of_epochs) with the original values as defaults. I also now pass the label argument through to ReviewDataset(filename, label=label), so I can train either the category model (label=0) or the sentiment model (label=1).

`1.10` Added the missing part regarding the harder part.
With the fixed training loop the final loss for the category model (label=0) is ` 0.2177`, while the final loss for the sentiment model (label=1) is `0.5688`. Since therefore, sentiment classification appears to be the harder task. The seed is needed to get the same random values we got first time  when training our model, to avoid having different output each time we train the model.

`1.12` I repeated the experiments with and without Kaiming initialisation and then compared the final losses.  
- Without Kaiming, the final loss was 0.2177 for the category model and 0.5688 for the sentiment model.  
- With Kaiming, the final loss dropped to 0.1453 for the category model and 0.4498 for the sentiment model.

So Kaiming initialisation clearly improves optimisation for both tasks in my setup (the loss is lower in both cases). This matches the idea that a better weight initialisation can make it easier for gradient descent to find good minima and leads to faster or more stable convergence.
## How I used AI
Ai was actually very helpful in many of those tasks especially to explain parts that were very confusing (like when it comes to the order of the steps, such as why we took this step first) Although I found out some AI modes that are useful such as learning mode which provides works in a way where it asks the users questions and make the conversation more interactive until the user figure out the solution.  


# Feedback on Lab 2
## Teacher feedback
`Task 2.01:` Good explanation. You correctly identified all hyperparameters and their purposes. To clarify, `n_head` is the number of heads in the MHA mechanism.

`Task 2.02: `Looks good. You found the correct minimum value and input, and provided a clear explanation of the key difference between GELU and ReLU regarding differentiability.

`Task 2.03:` Detailed annotations. However, note that after `x = self.c_fc(x)`, it is only the final dimension that has been multiplied by 4.

`Task 2.04: `Good job!

`Task 2.05: `Excellent documentation! Your line-by-line shape annotations with print statements clearly trace the tensor transformations through the attention mechanism.

`Task 2.06: `Well explained.

`Task 2.07: `You found the correct passage and summarised the benefits.

`Task 2.08: `Clear explanation of the key benefits.

`Task 2.09: `Looks good!

`Task 2.10:` Solid work! Your implementation correctly maps all pretrained weights to the model, including the transpose for linear layers and all transformer blocks.

`Task 2.11:` Your temperature scaling and top-k sampling are correctly implemented with proper masking logic.

`Task 2.12:` Looks good!

## My improvements

...

## How I used AI
The same as first lab