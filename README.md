# nerdLLama: Fine-tuning LLama2 with MultiNERD dataset

![1980724_1](https://github.com/naserahmadi/nerdLLama/assets/45039751/0dcae54b-eb72-438a-a85f-1ba8639c9782)

`nerdLLama` has been created by fine-tuning [LLama2-7B](https://huggingface.co/NousResearch/Llama-2-7b-hf) on english subset of [multiNERD](https://huggingface.co/datasets/Babelscape/multinerd?row=17) dataset.  

I Fine-tuned LLama2 using `QLora` for all linear attention layers as target modlues (`lora_rank=256` and `lora_alpha=256`).

# How to RUN

## The repository contains the following notebooks:
* `data_preparation` contains the code for chainging the format of tokens and ner tags. Here we use a natural language format for training our LLM. We fisr change the ner indices to their real tage (B-PER, I-PER, etc.) and then convert the lists into two natual sentences. E.g. The following example:

  ``  {"tokens": ["Each", "included", "ginseng", "."], "ner_tags": [0, 0, 25, 0], "lang": "en"} ``

Will be converted into: ``  instruction: Each included ginseng . output: ginseng:B-PLANT ``



* `llama2_train` first creates the instructions from dataset and uses them for fine-tuning our llama2 base model. As it was mentioned before, the fine-tuning is based on `QLora` (`15%` of the parameters are tainable). We use the following code to generate the fine-tuning dataset.

``
def create_text_row(instruction, output):
    text_row = f"""<s>[INST] {instruction} [/INST] \\n {output} </s>"""
    return text_row
``

* `llama2_test` contains the code for running the created model on `test-dataset`.

* `llama2_eval` contains the code for running the evaluations on the geneated results. We report `precision`, `recall`, and `fscore` to evaluate the performance of the model.

* Training data are saved in `data` folder and results are saved in `results`.


# The performance of the model:

Here we report the performance of our model for two scenarios: `SystemA` and `SystemB`. In each case, we train the model for two epochs and report the performance of each checkpoint. 

## SystemA results

|model name | epoch | train_loss | eval_loss | inference_time (s) | precision | recall | fscore |
|-----------------|:------:|:---------:|:---------:|:---------:|:---:|:---:|:---:|
|nerdLLama_systemA_1|1| | | | 95.59% | 96.36% | 97.14% !
|nerdLLama_systemA_2|2| | | | |  | |


## SystemB results

|model name | epoch | train_loss | eval_loss | eval_time_per_steps (s) | precision | recall | fscore |
|-----------------|:------:|:---------:|:---------:|:---------:|:---:|:---:|:---:|
|nerdLLama_systemB_1|1| 1.1177 | 1.105619192123413 | 8.527 | |  | |
|nerdLLama_systemB_2|2| 0.8606 | 1.188624143600463 | 8.493 | |  | |
