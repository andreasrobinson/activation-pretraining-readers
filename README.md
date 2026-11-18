# Activation prediction pretraining
Preliminary results (Nov 17, 2026). Full writeup in progress

![Hidden-word accuracy vs activation pretraining](top-1-hidden-word-trend.png)

*Figure:* This plot shows initial results with activation-prediction-based pretraining for improved activation interpretation and monitoring.  Fine-tuning results are shown with and without this pretraining, where the fine-tuning used a fixed 61k-pair self-supervised prompt-prediction task, ie learning to predict prompts from activations.  We can see from the plot that increased pretraining with activation prediction steadily improves the capacity to guess hidden words based on activations, in a model organism finetuned to keep 8 secret words.  With 4.3 billion activation tokens in pretraining, our activation reader gets ~60% accuracy at ranking the hidden word top-1 of the 8 options, versus 33% with no activation pretraining (with chance on these 8 words at 12.5%).  Also, we can see that the approach beats various standard baselines, including a tuned lens fitted on the model organism, as well as an English-only control with the same pretraining text but no activation-prediction objective, to verify that activation prediction was itself providing the benefit. 

## Clarifications and Caveats:

1. In addition to the simple prompt-prediction fine-tuning we also tried replicating the fine-tuning training data from the original activation oracle paper, but surprisingly the simple activation-to-prompt fine-tune task gave better results for this model/task, at this 0.6B scale.  
2. Note this activation pretraining approach is not specific to activation oracles, and it could be interesting to test this pretraining with other methods e.g. natural language autoencoders.
3. Also, the current activation reader (and model organism) is just a 0.6B Qwen model, which is quite small, and an upcoming priority is to test activation pretraining with larger models (e.g. 8B) to see how it scales with model size.
4. We currently test with one family of model organisms with 8 hidden words, where the current hidden word depends on LLM context; this reflects a realistic setting where the target model's deceptive behavior varies with context.
5. These tests are so far mostly with one seed each, so additional random seeds should be tested.
6. The above plots are with a fixed fine-tuning budget, and we have some encouraging data at varied fine-tuning scales, but further experiments will be needed to assess whether pretraining provides an advantage across fine-tuning scales or just a head-start in performance which could be offset with sufficient fine-tuning scale.

