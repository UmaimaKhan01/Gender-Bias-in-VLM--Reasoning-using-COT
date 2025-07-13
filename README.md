# Gender-Bias-in-VLM--Reasoning-using-COT
We propose an explainable system for detecting gender bias in vision-language models (VLMs) using real-world image-caption pairs and step-by-step reasoning. GPT-4 generates chain-of-thought justifications, and LLaMA-V-O1 is fine-tuned on structured reasoning data. This method offers rationale-backed decisions, ensuring ethical AI transparency.

![image](https://github.com/user-attachments/assets/735f6716-9081-4cab-a2e5-6e8bc836a43f)


![image](https://github.com/user-attachments/assets/04755eb9-a920-4d9c-a003-1fd6d66d6232)


In AI systems, detecting bias in multimodal image-caption pair data is a difficult and urgent
problem. When text or images overstate, misrepresent, or
emphasize particular parts of a story, bias may be evident. Models that are capable of determining and evaluating complex semantic links between textual and visual
material are necessary for effective detection. In order to
enhance bias recognition capabilities in vision language
models (VLMs), this research presents a methodology that
makes use of partially synthetic reasoning data. By making
models to infer the root causes of bias from image-caption
pairs. Our goal in the method goes beyond surface-level
bias identification by enabling models to infer the underlying reasons for biases from paired images and captions.
This research represents a comprehensive exploration of using Chain-of-Thought (CoT) data for bias recognition in
vision-language models
