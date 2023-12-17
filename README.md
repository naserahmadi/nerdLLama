# nerdLLama: Fine-tuning LLama2 with MultiNERD dataset

![1980724_1](https://github.com/naserahmadi/nerdLLama/assets/45039751/0dcae54b-eb72-438a-a85f-1ba8639c9782)

`nerdLLama` has been created by fine-tuning [LLama2-7B](https://huggingface.co/NousResearch/Llama-2-7b-hf) on english subset of [multiNERD](https://huggingface.co/datasets/Babelscape/multinerd?row=17) dataset.  

I Fine-tuned LLama2 using `QLora` for all linear attention layers as target modlues (lora_rank=256 and lora_alpha=256).

# How to RUN

## The repository contains the following notebooks:
* `data_preparation` contains the code for chainging the format of tokens and ner tags. Here we use a natural language format for training our LLM. We fisr change the ner indices to their real tage (B-PER, I-PER, etc.) and then convert the lists into two natual sentences. E.g. The following example:

  ``  {"tokens": ["Each", "included", "ginseng", "."], "ner_tags": [0, 0, 25, 0], "lang": "en"} ``

Will be converted as:

``  Each included ginseng . ginseng:B-PLANT ``



