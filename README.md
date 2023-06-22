# InstructionFr
InstructionFr is a repository of instruction dataset in French to finetune LLMs curated by OpSci.

Following the release of LLama in February 2023, there has been a swift development of instruction datasets to fine-tune LLMs for conversational uses or on specific tasks. Yet theses dataset are largely focused on English and to a lesser extent on Chinese. 

This project strives to compile the open instructions datasets that are either under a free license, in the public domain or generated.
* **L'Oracle** 4,613 questions and answers from French Wipedia (l'Oracle: a community space for solving tricky encyclopedic questions). Like all the texts on Wikipedia, theses are published under a CC-By-SA license.
* **Novel17** 6000 excerpts of French novels in the public domain from 1600 to 1700. We have mostly used Google Books since it has a better historical OCR than alternative sources (like Gallica).
* **Novel19** 5598 excepts of French novels in the public domain from 1815 to 1940.
* **Vicogne** 68,359 instructions automatically translated from common instruction datasets (Alpaca, ShareGPT…) by the Vicogne Project. A smaller subset of 6000 instructions has been made available.
* **FrenchInstruction** XXXX instructions natively written and generated in French extracted from leading instructions datasets (Guanaco, ShareGPT, OpenAssistant, Self-Instruct…)

# Usage
Theses datasets aim primarily to serve as reusable educational resources for training open LLMs. Diversity and free reuse were important requirements.

The **Novel17** is a good illustration of the dramatic impact of fine-tuning. The language is basically a variant of French with a specific syntax, vocabulary and even letters (the "long" s). In less than one epoch (roughly 15 minutes on a A100 with 40 go of VRAM), Falcon-Instruct-7B assimilate already very well theses specificities :



It's advisable to use to use a slightly higher learning rate in this case. As expected 
