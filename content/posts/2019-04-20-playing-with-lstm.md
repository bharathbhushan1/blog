---
title: "Playing with LSTM"
date: 2019-04-20T10:00:00
draft: false
tags: ["machine learning", "LSTM"]
---

I was trying to learn the basics of LSTM and found this [pytorch tutorial](https://pytorch.org/tutorials/beginner/nlp/sequence_models_tutorial.html). Starting from there I tried out the parts-of-speech tagger with minor modifications.


### 1. Import the necessary libraries
```
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
import numpy as np

torch.manual_seed(1)
```
<!--more-->

### 2. Create the LSTMTagger neural network

```
class LSTMTagger(nn.Module):
    def __init__(self, embedding_size, hidden_layer_size,
                 vocab_size, part_of_speech_size):
        super(LSTMTagger, self).__init__()
        self.embedding = nn.Embedding(vocab_size, embedding_size)
        self.lstm = nn.LSTM(embedding_size, hidden_layer_size)
        self.linear = nn.Linear(hidden_layer_size, part_of_speech_size)

    def forward(self, sentence):
        o1 = self.embedding(sentence)
        o2, _ = self.lstm(o1.view(len(sentence), 1, -1))
        o3 = self.linear(o2.view(len(sentence), -1))
        part_of_speech_scores = F.log_softmax(o3, dim=1)
        return part_of_speech_scores
```


### 3. Original training and test data
```
training_data = [
    ("The dog ate the food".split(),
         ["DET", "NOUN", "VERB", "DET", "NOUN"]),
    ("Everyone read the book".split(),
         ["NOUN", "VERB", "DET", "NOUN"]),
    ("Dogs are cute".split(),
         ["NOUN", "DET", "ADJECTIVE"]),
    ("This is a cute couple".split(), 
         ["DET", "DET", "DET", "ADJECTIVE", "NOUN"]),
    ("Dogs are faithful".split(), 
         ["NOUN", "DET", "ADJECTIVE"]),
]

test_data = [
    ("This is a cute dog".split(), 
         ["DET", "DET", "DET", "ADJECTIVE", "NOUN"]),
    ("Everyone ate the food".split(), 
         ["NOUN", "VERB", "DET", "NOUN"])
]
```

### 4. Prepare metadata related to the data
```
def prepare_sequence(seq, to_ix):
    idxs = [to_ix[w] for w in seq]
    return torch.tensor(idxs, dtype=torch.long)

word_to_ix = {}
for sent, tags in training_data:
    for word in sent:
        if word not in word_to_ix:
            word_to_ix[word] = len(word_to_ix)
print(word_to_ix)
tag_to_ix = {"DET": 0, "NOUN": 1, "VERB": 2, "ADJECTIVE": 3}

# p unique words are in the vocabulary
vocab_size = len(word_to_ix)        

# q unique parts of speech are present
part_of_speech_size = len(tag_to_ix)

# tag_to_ix and word_to_ix are used to create the numeric
# training and test data
```

_Output_
```
{'a': 13, 'book': 7, 'ate': 2, 'couple': 14, 'faithful': 15, 'cute': 10, 'food': 4, 'is': 12, 'This': 11, 'dog': 1, 'Dogs': 8, 'read': 6, 'Everyone': 5, 'are': 9, 'the': 3, 'The': 0}
```

### 5. Train the model
```
# the word embedding is a n-dim vector
embedding_dim = 5       

# the LSTM hidden layer has m LSTM neurons
hidden_dim = 4          

model = LSTMTagger(embedding_dim, hidden_dim,
                   vocab_size, part_of_speech_size)
loss_function = nn.NLLLoss()
optimizer = optim.SGD(model.parameters(), lr=0.1)

with torch.no_grad():
    inputs = prepare_sequence(test_data[1][0], word_to_ix)
    part_of_speech_scores = model(inputs)
    print("SCORE BEFORE: ", np.argmax(part_of_speech_scores, 1))

for epoch in range(200):
    for sentence, parts_of_speech in training_data:
        model.zero_grad()
        sentence_vector = prepare_sequence(sentence, word_to_ix)
        part_of_speech_targets = prepare_sequence(parts_of_speech,
                                                  tag_to_ix)
        part_of_speech_scores = model(sentence_vector)
        loss = loss_function(part_of_speech_scores,
                             part_of_speech_targets)
        loss.backward()
        optimizer.step()
```

_Output_
```
('SCORE BEFORE: ', tensor([0, 0, 0, 0]))
```

### 6. Test the model
```
with torch.no_grad():
    inputs = prepare_sequence(test_data[1][0], word_to_ix)
    part_of_speech_scores = model(inputs)
    part_of_speech_targets = prepare_sequence(test_data[1][1], tag_to_ix)
    print("TARGETS", part_of_speech_targets)
    print("SCORE AFTER: ", np.argmax(part_of_speech_scores, 1))
```

_Output_
```
('TARGETS', tensor([1, 2, 0, 1]))
('SCORE AFTER: ', tensor([1, 2, 0, 1]))
```

### Note
* This approach has problem w.r.t packing. If you want to pack the inputs, then you need to be careful and do something like [this](https://gist.github.com/Tushar-N/dfca335e370a2bc3bc79876e6270099e).
* Interesting post: https://towardsdatascience.com/taming-lstms-variable-sized-mini-batches-and-why-pytorch-is-good-for-your-health-61d35642972e
* Interesting post: https://medium.com/@sonicboom8/sentiment-analysis-with-variable-length-sequences-in-pytorch-6241635ae130
