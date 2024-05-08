# Task

NumEval - task2

It is centered on the cloze test, where models are required to identify the correct numerical value from four given options, based on a provided news article. The dataset utilized for this task is NQuAD, which is in Chinese. 

![img](https://lh7-us.googleusercontent.com/vOJlpPTYbqgR1xVf_HHC6aeSBD-jilGgiWeNT7B9X9ETfspJKJdQFQGmCZjPFJM2EzTBmid_zNMnJevNHKFpwUgwD30aBE3nSRlCBiYgO_I7nP1vCLziUhfi2ENz3rM8QGK4nc0U3clzaVX20KA_SDk)

Ref: https://sites.google.com/view/numeval/numeval?authuser=0

Paper: https://sites.google.com/view/numeval/numeval?authuser=0#h.kjhrdjtve3l

# Data

NQuAD

https://drive.google.com/file/d/16-a6d8FtGp17W8_l4eYd-h42NlvQJW7w/view

The dataset for bert, gpt is modified based on NQuAD. The format is as follows:

| **context:** Major banks take the lead … from fully starting. |
| :----------------------------------------------------------- |
| **ans0:** Driven by self-discipline, … are approaching nearly 0.04% |
| **ans1:** Driven by self-discipline, … are approaching nearly 1.986% |
| **ans2:** Driven by self-discipline, … are approaching nearly 2% |
| **ans3:** Driven by self-discipline, … are approaching nearly 2.5% |
| **ans:** 2                                                   |

# Model

## Nemo

<u>nemo_model.ipynb</u>

This model is an implementation based on the architecture proposed in NumEval-task2. We've reconstructed the model to process numerical questions effectively, focusing on two main components:

1. **Context Encoder (CE)**: Utilizes BERT-Large to generate embeddings for question stems and related sentences, enhancing them with additional contextual and numerical information.
2. **Numeral Encoder (NE)**: Encodes the numerical answer options (A, B, C, D).

Both parts integrate through a BiGRU network to capture sequential data dependencies, culminating in a robust representation that feeds into an MLP for answer prediction. This setup is ideal for complex QA tasks requiring deep textual and numerical understanding.

## BERT

<u>Bert.ipynb</u>

Ref: https://huggingface.co/docs/transformers/tasks/multiple_choice

To improve accuracy, We fine-tune BERT, RoBERTa for multiple-choice task.  `DataCollatorForMultipleChoice`  is employed to dynamically adjust variable input sequence for batch processing. Our configuration includes periodic evaluations after each training epoch, utilizes a learning rate of 2e-5, and applies a weight decay of 0.01 to prevent overfitting. We use the AdamW optimizer to efficiently manage learning and resource use across three training epochs.

## GPT

<u>GPT.ipynb</u>

Ref: https://platform.openai.com/docs/guides/fine-tuning

`baggage-002` is used for training multiple-choice tasks. `baggage-002` is a replacement for the GPT-3 ada and baggage base model. We transform the dataset into ‘prompt-completion’ format. We also use the OpenAI CLI tool to adjust the format. The 'prompt-completion' format is as follows:

| *“Prompt”*: “Major banks take the lead … from fully starting. Choose the sentence that best summarizes the article from the four options below, considering the accuracy of the numbers in the sentence. Options: A) Driven by self-discipline, … are approaching nearly 0.04%. B) Driven by self-discipline, … are approaching nearly 1.986%. C) Driven by self-discipline, … are approaching nearly 2%. D) Driven by self-discipline, … are approaching nearly 2.5% ->” |
| ------------------------------------------------------------ |
| *“Completion”:* “C”.                                         |

# Result

|          | NEMo   | XGBoost | BERT   | Roberta | GPT    |
| -------- | ------ | ------- | ------ | ------- | ------ |
| Accuracy | 46.07% | 46.53%  | 81.46% | 82.33%  | 74.90% |