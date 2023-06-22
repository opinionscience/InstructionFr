# InstructionFr
InstructionFr is a repository of instruction dataset in French to finetune LLMs curated by OpSci.

Following the release of LLama in February 2023, there has been a swift development of instruction datasets to fine-tune LLMs for conversational uses or on specific tasks. Yet theses dataset are largely focused on English and to a lesser extent on Chinese. 

This project strives to compile the open instructions datasets that are either under a free license, in the public domain or generated.
* **L'Oracle** 4,613 questions and answers from French Wipedia (l'Oracle: a community space for solving tricky encyclopedic questions). Like all the texts on Wikipedia, theses are published under a CC-By-SA license.
* **Novel17** 6000 excerpts of French novels in the public domain from 1600 to 1700. We have mostly used Google Books since it has a better historical OCR than alternative sources (like Gallica).
* **Novel19** 5598 excepts of French novels in the public domain from 1815 to 1940.
* **Vicogne** 68,359 instructions automatically translated from common instruction datasets (Alpaca, ShareGPT…) by the Vicogne Project. A smaller subset of 6000 instructions has been made available.
* **FrenchInstruction** 9083 instructions natively written and generated in French extracted from leading instructions datasets (Guanaco, ShareGPT, OpenAssistant, Self-Instruct…)

All the instructions follow the classic Alpaca structure:

```json
{"instruction_id":"20230423192100-Affluent_de_la_Seine?","instruction":"Bonjour à vous, Je lisais l'article sur la Tour de Nesle et ai cliqué sur la première carte de l'article [[2]]. Juste à côté de la Tour, il y a un pont qui enjambe... qui enjambe quoi? J'ai regardé d'autres cartes et n'ai pas trouvé la réponse. La phrase sur une sorte de butte inondée par le fleuve en hiver ne m'aide pas. Si c'est une rivière, quelle rue actuelle la recouvre aujourd'hui? Formule cordiale,","input":"","output":"D'après le texte joint au dessin d'Eugène Viollet le Duc : « La vue perspective de cette tour (fig. 61), prise en dehors de la porte de Nesle, en fait comprendre la valeur comme poste d'observation sur le fleuve. De là des signaux pouvaient être transmis au Louvre , et vice versa, sur tout le front occidental des remparts de la rive gauche et au palais de la Cité. En amont de Paris, deux autres tours à peu près semblables à celle-ci barraient la rivière: l'une, dite tour Barbeau, formait tête du rempart sur la rive droite; l'autre, dite la Tournelle, avait la même destination sur la rive gauche. Ces deux ouvrages, qui se trouvaient au droit du milieu de l'île Saint-Louis, se reliaient avec deux autres tours élevées sur les berges de cette île, coupée alors par un fossé que remplissait la Seine ... » Ce serait donc la Seine elle-même.","source":"Oracle (fr.wikipedia.org)"}
```


# Usage
Theses datasets aim primarily to serve as reusable educational resources for training open LLMs. Diversity and free reuse have been important requirements.

The **Novel17** is a good illustration of the dramatic impact of fine-tuning. The language is different standard than contemporary French with a specific syntax, vocabulary and even letters (such as the "long" s, ſ). In less than one epoch (roughly 15 minutes on a A100 with 40 go of VRAM), Falcon-Instruct-7B assimilate already theses specificities :

> *Instruction* : Plus d'une année après avoir dépoſé mon doſſier en ligne en avril 1622 à la préfecture de Nanterre, je n'ai toujours point reçu la validation de mon doſſier. (Nous ſommes en mai 1623)
> *Output (Novel17 fine-tuned)* : Vous ſerez informé par les lettres que j'ay reçues à quel point j'ai été retardé dans cette affaire par l'incurrence de l'hiver & de quelques autres difficultés & que j'ay ſurpris ce long retard de ſon deſpéter l'avancement de ſes affaires. Je vous ſouffre en ce moment d'une grande affliction qui m'a été conſignéë par mon ſouffleur à qui j'ay donné de grandes marques de ſubordination. Ce que j'ay à vous dire de plus ſubstantiel en cette matière c'eſt que j'ay été ſurpris d'un enchaîniment de négligence & d'inconſtance de celui qui eſt chargé de faire la ſecrété à la Cour qui vous eſt : Vous vous ſerez donc bien plaire de ne me point accuser de la négligence dont j'ay été victime & dont j'ai bien du regret. Je vous supplie de me faire grace & de me donner quelque faveur qui me permettre de venir à vous au lieu que mon ſouffleur me diſoit que je ne pouvois pas venir à Paris avant un an. 

Despite explicit prompting, GPT-4 is unable to provide a valid reconstruction of 17th century French:
>*Instruction*: Est-ce que tu peux écrire la réponse à cette demande dans le style et l'orthographe du français du 17e siècle : "Plus d'une année après avoir dépoſé mon doſſier en ligne en avril 1622 à la préfecture de Nanterre, je n'ai toujours point reçu la validation de mon doſſier. (Nous ſommes en mai 1623)"
>*Output (GPT-4)* Cher Monsieur/Madame, Salutations respectueuses, je me permets de porter à votre connaissance ma grande préoccupation quant à l'état actuel de mon dossier. En effet, il m'a été loisible de déposer ledit dossier électroniquement au sein de votre honorable préfecture, lors du mois d'avril en l'an de grâce 1622. Malheureusement, je suis au regret de vous informer que, jusqu'à ce jour, le sceau précieux de votre approbation n'a point été apposé sur mes parchemins.

To fine-tune on unusual datasets like novel17, it is advisable to use to use a slightly higher learning rate in this case. As expected you will also get a highest loss value (the error rate) since the model will struggle more to accomodate the new data.
