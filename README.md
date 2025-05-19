
# Testing Multilingual Robustness on Causal Tracing

## Introduction
Large-scale autoregressive language models such as GPT have revolutionized natural language understanding and generation by internalizing vast amounts of factual knowledge from their training corpora. However, despite their remarkable capabilities, fundamental questions remain about how exactly these models store, access, and utilize factual information within their complex neural architectures. More crucially, can we precisely locate and manipulate specific pieces of knowledge inside these models without destabilizing the rest of their learned representations?

This project builds upon and extends the groundbreaking research by Meng et al., who pioneered causal tracing and model editing techniques to identify where factual knowledge resides in GPT-style models and how targeted interventions can alter model behavior predictably. Their findings reveal a striking functional dichotomy within transformer architectures: a clear separation between the neural substrates responsible for “knowing” facts and those responsible for “expressing” them. Specifically, Meng et al. identified two key sites of causal influence on factual predictions: 1. The Early Site, located near the end of the subject phrase within the input sequence, where MLP (feed-forward) modules in transformer layers predominantly encode factual knowledge. 2. The Late Site, situated just before output token generation, where self-attention mechanisms primarily govern the articulation of known facts into fluent text.

Disabling or intervening in the MLP modules drastically impairs the model’s ability to retrieve factual information, underscoring their critical role as the repository of world knowledge. Conversely, interventions at the attention modules influence how facts are verbalized without erasing the underlying knowledge. This distinction between “knowing” and “saying” facts not only aligns with longstanding theories in cognitive science and philosophy but also offers a principled framework for precise model editing. By selectively targeting the early site, one can modify or correct specific factual associations—potentially enabling safer, more efficient updates to large language models without full retraining.

## Testing Robustness with Chinese and German Prompts
To evaluate the robustness and generality of causal tracing and factual editing techniques, this project extends the original methodology to include CHinese and German-language factual prompts. While most previous work has focused exclusively on English inputs, real-world applications demand that models operate reliably across languages.

we can probe whether:

1. The same early vs. late site dichotomy appears in non-English inputs.

2. The localization of knowledge in the MLP modules remains consistent under different tokenizations.

3. The model’s editing response is language-agnostic, or if architectural biases exist favoring English.

This multilingual extension also allows us to investigate whether Bloom models internalize knowledge in a language-independent conceptual space, or if language-specific representations exist that influence editing performance.


## Results
### Reproduce Results
I try to reproduce the result using the same prompt as the original paper: "The Space Needle is in the city of"
<p align="center">
<img src="https://github.com/user-attachments/assets/ca1d1df6-fc04-4eb5-a517-ece5041f5900" width="500" height="300"/>
</p>
The heatmap turns out to be very similar or I could say the same as the GPT-series model, indicating that the early and late site phenomenon not only happens on GPT series models but also applies to the Bloom7B model.

### To be fixed!

As for the MLP and Attention Layers, i believe my results are not correct, although the layers seem to be correct during the debug process. The MLP and Attention Layers experiments cannot reproduce the results of the original paper, the main reason of this error might be due to the different definition of layer name between GPT series models and Bloom which is called from transformer library. Or maybe the "trace_with_batch" function didn't apply noises correctly on the hiddenstates, leading to impossible to reproduce. Below are the results of MLP and attention layers, to be mentioned, the two results are the same, so obviously something went wrong.


<p align="center">
<img src="https://github.com/user-attachments/assets/a6fce132-914c-4e7d-b9a8-b9f33a637647" width="500" height="300"/>
<img src="https://github.com/user-attachments/assets/05d16f58-848b-43a5-a5ae-87d5784bca16" width="500" height="300"/>
</p>

### German Prompts Results
The German prompts results are robust, although the early site is not as obvious as the English prompts, consistent and distinct early & late site phenomenon can be observed in German version.
<p align="center">
<img src="https://github.com/user-attachments/assets/e08dc490-60ca-4efe-8667-05603bfae8d7" width="500" height="300"/>
<img src="https://github.com/user-attachments/assets/c052fdc1-232e-4146-b1ac-79e758c73284" width="500" height="300"/>
</p>

## Usage
Please refer to the colab notebook named "causal_trace_(1).ipynb", in which I adapt the Bloom7B model into the framework and demonstrate how a heatmap can be generated. The notebook is modified based on the original demonstration script, which are in the notebook category.

## More could be done
This project only try to reproduce the results shown in the paper and extend the framework to a multilingual aspect, however haven't place attention on ROME model, which is used to edit the fact storage within a model.

