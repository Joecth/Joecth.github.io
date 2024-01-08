---
layout: post
categories: LLM
tag: []
date: 2023-07-31
Author: Jo
---



# Fine-Tuning LLM with Instructions

## Abstract



通過一種名為「指令微調」的過程來調整基礎語言模型，例如FLAN-T5。在此過程中，使用了特定的提示模板和數據集來進行訓練，並利用了如 ROUGE 和 HELM 等評估指標來衡量微調的成功程度。指令微調證明在廣泛的自然語言任務中非常有效，只需幾百個範例，就能根據特定任務進行精細調整。

您還學習了兩種參數高效微調（PEFT）方法——LoRA和提示調整，這些方法可以減少微調模型所需的計算量。在實踐中，將LoRA與量化技術結合（稱為QLoRA）可進一步減少內存使用。PEFT策略大量用於最小化計算和內存資源，進而降低微調成本，讓您可以充分利用計算預算，並加快開發流程。

1. 指令 fine-tuning 是讓模型能夠更有效地回應我們的提示或問題。這種方法從大量的互聯網文本學習，並在更小的數據集上進行微調，以學習跟隨指令。然而，需要注意的是慘慘遺忘 (catastrophic forgetting)，即模型可能忘記之前學到的大部分內容。進行指令 fine-tuning 時，需要涵蓋多種不同的指令類型，以防止模型忘記之前學到的內容。
2. 參數效率 fine-tuning (PEFT) 允許開發者將模型特化為特定的應用，而無需對模型的每一個參數進行微調。這種方法可以保持原始模型的權重不變，或者在模型之上添加適應性層，這樣就可以減少記憶體使用，而且仍然可以獲得與全模型 fine-tuning 相似的效果。其中，一種受到廣泛使用的技術是 LoRA，它可以使用低秩矩陣來獲得相當好的性能結果，同時也減少了計算和記憶體需求。

兩種方法都有其優點和使用場景，但是在實際操作中，許多開發者會選擇先從 prompting 開始，如果性能達到滿意的水平，就直接使用；如果效果不佳，則使用 PEFT 或 LoRA 等 fine-tuning 技術來提高性能。但需要注意，使用巨大模型的成本是一個值得討論的問題，有時候为特定应用 fine-tuning 一个较小的模型会更有利。



## Instruction Fine-Tuning

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230802153201434.png" alt="image-20230802153201434" style="zoom:33%;" />



## Prompt Eng, ICL，(In-context Learning)

| 類型                                                 | 描述                             | 優點                                                 | 缺點                                                         |
| ---------------------------------------------------- | -------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------ |
| 零次學習（Zero-shot inference）                      | 模型能識別提示中的指令並正確執行 | 對於某些模型（通常是較大的模型）來說，能直接完成指令 | 對於較小的模型，可能無法正確完成任務                         |
| 單次學習或少次學習（One-shot or Few-shot inference） | 包含一個或多個想要模型完成的示例 | 可以幫助模型識別任務並生成良好的完成情況             | 1. 對於較小的模型，即使包含五到六個示例，也不一定能正確完成任務 2. 您在提示中包含的任何示例都會占用上下文窗口中的寶貴空間，減少了您可以包含其他有用信息的空間 |

However, this strategy has a couple of drawbacks. 

1. First, for smaller models, it doesn't always work, even when five or six examples are included. 
2. Second, any examples you include in your prompt take up valuable space in the context window, reducing the amount of room you have to include other useful information.

所以可以用Fine-Tuning想辦法解決



## Fine-Tuning

- To improve the performance and adaptability of a pre-trained language model for specific tasks.

| 學習方式                                      | 描述                                                         | 優點                                           | 缺點                                                         |
| --------------------------------------------- | ------------------------------------------------------------ | ---------------------------------------------- | ------------------------------------------------------------ |
| Prompt-engineering                            | 透過在提示中包含一個或多個示例，來幫助模型識別任務並產生良好的完成內容（也稱為一擊或少數擊推理） | 可以在不需要進行微調的情況下改善模型的任務性能 | 1. 對於較小的模型，即使包含五或六個示例，也不一定總是有效 2. 您在提示中包含的任何示例都會占用寶貴的上下文窗口空間，減少了您包含其他有用信息的空間 |
| Pre-training                                  | 使用大量非結構化的文本數據進行自監督學習來訓練LLM            | 可以讓模型獲得廣泛的語言知識，理解各種語境     | 1. 需要大量的數據 2. 對於特定任務，模型的性能可能不佳        |
| **Fine-tuning** (**Instruction Fine-tuning**) | 透過監督學習的方式，使用標記過的數據集（提示和完成對）來更新LLM的權重; 利用指令來訓練模型，讓模型學習如何對特定指令進行回應 | 可以改善模型在特定任務上的表現                 | 需要有標記過的數據集，且這些數據集必須和目標任務相關; 同樣需要有含有特定指令的標記過的數據集 |

在「Fine-Tuning」過程中，模型會使用包含特定指令的示例進行訓練。如：若欲改善模型的摘要能力，便需要構建包含 "Summarize the following text" 或類似語句的資料集。



### AWS sample prompt instruction termplates

<img src="https://p.ipic.vip/x3o1gs.png" alt="image-20230802154918259" style="zoom: 25%;" />



<img src="https://p.ipic.vip/0sj0it.png" alt="image-20230802155435048" style="zoom:25%;" />



當我們使用LLM，它的輸出是跨令牌的概率分佈。 因此，我可以<u>比較completion的分佈和訓練標籤的分佈</u>，並使<u>用標準交叉熵函數</u>
<u>來計算兩個令牌分佈之間的損失。 然後，我會使用這個計算出的損失來在標準反向傳播過程中更新模型的權重。 我將對許多批次的提示完成對進行這樣的操作，並在幾個時期內更新權重，以提高模型在任務上的性能。</u> 就<u>像標準的監督學習一樣</u>，我可以定義單獨的評估步驟，使用保留的驗證數據集來衡量我們的LLM的性能。 這將為我提供驗證準確性，並且在完成微調後，我可以使用保留的測試數據集進行最終的性能評估。 這將為我提供測試準確性。 微調過程將產生一個新版本的基本模型，通常被稱為指令模型，它更適合我們感興趣的任務。使用指令提示進行微調是當今微調LLM的最常見方法。指的都是「Instruction Fine-Tuning」





## On a Single Task / Multi-Tasks

| 微調方式          | 描述                                                         | 優點                                                         | 缺點                                                         |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 單任務微調        | 預訓練模型被微調以改善在特定任務上的性能。例如，使用一個特定任務的範例數據集來進行微調。 | 對於特定的任務，性能可以被大幅提升。相對少量的範例（500-1000個）就可以得到良好的效果。 | 這種方法可能導致"災難性遺忘"，使得模型在其他任務上的性能下降。 |
| 多任務微調        | 在多個任務上同時進行微調，以維持模型的多任務泛化能力。       | 保持了模型的多任務泛化能力，可避免災難性遺忘。               | 需要更多的範例（可能需要50-100,000個跨多個任務的範例）和更多的計算資源。 |
| 參數高效微調 PEFT | 這是一種只訓練少量任務特定適配層和參數的技術，大部分的預訓練權重都不變。 | 對災難性遺忘顯示出更大的抵抗力，因為大部分的預訓練權重都沒有改變。 | 這是一個仍在研究中的方法，可能還未完全成熟或有其它未知的挑戰。 |



### Multi-Tasks

> FLAN, which stands for **fine-tuned language net**, is a specific set of instructions used to fine-tune different models. Because they're FLAN fine-tuning is the last step of the training process the authors of the original paper called it the ***metaphorical dessert*** to the main course of pre-training quite a fitting name. 
>
> - FLAN-T5, the FLAN instruct version of the T5 foundation model 
> - FLAN-PALM is the flattening struct version of the palm foundation model. 

### <img src="/Users/joe/Library/Application Support/typora-user-images/image-20230802162252721.png" alt="image-20230802162252721" style="zoom:33%;" />

|                | FLAN                                                         | FLAN-T5                                                      | FLAN-PALM                                     |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| 定義           | Fine-tuned Language Net，是一個透過多任務指令微調的模型系列。不同的 FLAN 模型依據微調過程中使用的數據集和任務有所不同。 | FLAN-T5 是基於T5基礎模型的 FLAN 模型，它總共在 473 個數據集上進行了微調，涵蓋了 146 種任務類別。 | FLAN-PALM 是基於PALM基礎模型的 FLAN 模型。    |
| 用途           | 用於對不同的基礎模型進行微調。                               | 是一個適合通用的指令模型，展示出在多種任務上的良好性能。     | 類似於FLAN-T5，對不同的任務有不同的性能表現。 |
| 需要的範例數量 | 取決於微調的任務數量和種類，可能需要大量數據。               | 依據不同任務的需求，可能需要大量數據。                       | 依據不同任務的需求，可能需要大量數據。        |



### Datasets

|          | SAMSum                                                       | DialogSum                                                    |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 定義     | SAMSum 是一個專門用於對話摘要的數據集，包含16,000個類似於即時通訊的對話與摘要。 | DialogSum 是一個用於客服對話摘要的數據集，包含超過13,000個客服聊天對話和摘要。 |
| 用途     | 訓練語言模型如何總結對話，特別是友誼或日常生活中的對話。     | 訓練語言模型如何總結客服對話，並理解顧客的需求和問題。       |
| 對話內容 | 主要是朋友之間的日常對話。                                   | 主要是客戶與客服之間的對話。                                 |
| 設計目標 | 提供語言學家編寫的高品質對話和摘要數據集，以訓練語言模型。   | 提供實際的客服對話數據，以便語言模型能學習如何摘要對話並理解顧客的需求。 |

> SAMSum 是 "A System-Agnostic Method for Dialogue Summarization" 的縮寫。這個名字來自於創建該數據集的相關研究論文的標題。這個名字反映了數據集的設計目標，即為不同的對話摘要系統提供訓練資料，這些系統可以獨立於特定的對話系統進行運作。這意味著，無論對話是通過什麼媒介（例如即時消息，電子郵件等）或在什麼上下文中（例如客服，個人聊天等）進行，SAMSum 都應該能夠提供有用的訓練數據。



## Model Evaluation

|              | ROUGE                                                        | BLEU                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **全稱**     | Recall-Oriented Understudy for Gisting Evaluation            | Bilingual Evaluation Understudy                              |
| **使用情境** | 主要用於評估自動生成的摘要的品質                             | 主要用於評估機器翻譯的品質                                   |
| **衡量方式** | 1. ROUGE-N: 計算預測和參考文本間的 n-gram 重疊數 <br> 2. ROUGE-L: 計算最長共享子序列 (LCS) <br> 3. ROUGE-S: 計算跳躍的二元組（Skip-bigram）的重叠程度 | 1. BLEU: 針對多個不同大小的 n-grams 計算精確度 (precision)，並使用特定的平均方式進行整合 |
| **優點**     | 提供多種衡量方式（如單詞重疊、最長共享子序列等），更適合評估摘要生成的效果 | 考慮了多個不同大小的 n-grams 的精確度，更適合評估翻譯的品質  |
| **缺點**     | 可能會對高頻詞給予過高的權重，並可能無法有效捕捉詞序信息     | 無法考慮語句的語義和語境，並可能對詞序變動的語句給予過低的評分 |



### ROUGE, (Recall-Oriented Understudy for Gisting Evaluation）

ROUGE 是一種評估文本摘要（如從長篇文章中生成簡短摘要）的質量的方法。

ROUGE的重要概念包括「召回率」（Recall）、「精確度」（Precision）和「F1分數」（F1 score）。以下是這三種評估方法的基本概念：

1. 召回率：生成摘要與原文參考摘要中共享的詞語（或n-gram，即n個連續詞語的序列）的數量除以參考摘要中的詞語（或n-gram）的數量。
2. 精確度：生成摘要與原文參考摘要中共享的詞語（或n-gram）的數量除以生成摘要中的詞語（或n-gram）的數量。
3. F1分數：召回率和精確度的調和平均數，它平衡了召回率和精確度的重要性。

讓我們來看一個例子。假設我們有以下參考摘要（由人類撰寫）：

參考摘要："我喜歡喝茶。"

然後模型生成了以下摘要：

生成摘要："我真的很喜歡喝茶。"

在這種情況下，我們將會看到以下結果：

1. 召回率：所有的參考摘要中的詞（我，喜歡，喝，茶）都在生成摘要中出現了，因此召回率為 4/4 = 1。
2. 精確度：生成摘要中的四個詞（我，真的，很喜歡，喝，茶）中，有四個詞出現在參考摘要中，因此精確度為 4/5 = 0.8。
3. F1分數：F1分數是召回率和精確度的調和平均數，其公式為 2*(Recall * Precision) / (Recall + Precision)。在這個例子中，F1分數為 2*(1 * 0.8) / (1 + 0.8) = 0.88。

以上就是 ROUGE 指標在單詞級別（或稱為unigram）的運作方式，這通常被稱為 ROUGE-1。但是，ROUGE 也可以在更大的文本塊（如二元組，也就是兩個連續的詞，或者更大的n-gram）上運作。在這些情況下，我們將計算生成摘要和參考摘要中共享的n-gram的數量，並相應地計算召回率、精確度和F1分數。這被稱為 ROUGE-N，其中N代表n-gram的大小。例如，對於二元組（bigrams），我們會使用 ROUGE-2。此外，還有一種稱為 ROUGE-L 的評價指標，它計算生成摘要與參考摘要之間的最長公共子序列（LCS）。



### BLEU, (Bilingual Evaluation Understudy)

BLEU（Bilingual Evaluation Understudy）評分是一種常用來評估機器翻譯系統的效能的方法。BLEU評分的主要思想是：如果機器翻譯的結果越接近人類翻譯的結果，那麼該翻譯的品質就越高。

BLEU評分主要依賴於n-gram精確度。n-gram是一種語言模型，用於預測句子中的下一個詞。n-gram中的"n"表示我們要考慮的詞的數量。例如，"bigram"（二元組）考慮兩個相鄰的詞，"trigram"（三元組）考慮三個相鄰的詞，以此類推。

值得注意的是，BLEU評分還包括一個稱為"brevity penalty"（簡短處罰）的因素。如果機器翻譯的結果比參考翻譯短，那麼這個處罰因素會降低BLEU評分，以反映出這種短度差異。

舉一個例子，假設我們有一句英文 "The cat sat on the mat"，並且有一個參考翻譯為法語的 "Le chat s'est assis sur le tapis"。如果機器翻譯的結果是 "Le chat sur le tapis"，那麼這個翻譯的BLEU評分將會較低，因為它缺少了 "s'est assis"（坐下）這個動作。



## Benchmarks

| 基準測試 (Benchmark)                             | 年份 | 優點                                                       | 缺點                                                         |
| ------------------------------------------------ | ---- | ---------------------------------------------------------- | ------------------------------------------------------------ |
| GLUE (General Language Understanding Evaluation) | 2018 | 能夠評估模型在多種任務上的通用能力。                       | 由於該基準測試涵蓋範疇廣泛，可能無法針對特定領域的任務進行深入評估。 |
| SuperGLUE                                        | 2019 | 包含更具挑戰性的任務，可以評估模型在更複雜情境下的表現。   | 雖然難度提高，但可能仍無法完全捕捉到某些具有高度專業知識或創造力要求的任務的表現。 |
| MMLU (Massive Multitask Language Understanding)  | 2021 | 能夠評估模型在多個學科領域的知識和問題解決能力。           | 由於範圍非常廣泛，可能對某些特定領域的深度評估仍然有限。     |
| BIG-bench                                        | 2022 | 提供極其多樣化的任務，包括語言學、數學、物理學等多個領域。 | 由於範疇龐大，可能難以精確定位模型在特定任務上的弱點。此外，進行這種大型基準測試可能需要較高的計算成本。 |
| HELM (Holistic Evaluation of Language Models)    | N/A  | 能夠全面評估模型，並提供了對公平性、偏見和有害行為的評估。 | 雖然它全面，但可能仍然無法完全捕捉到模型在特定任務或場景下的特定優點或缺點。 |





# PEFT, (Parameter efficient fine-tuning)

> Full fine-tuning results in a new version of the model for every task you train on. Each of these is the same size as the original model, so it can create an expensive storage problem if you're fine-tuning for multiple tasks. Let's see how you can use PEFT to improve the situation. With parameter efficient fine-tuning, you train only a small number of weights, which results in a much smaller footprint overall, as small as megabytes depending on the task. The new parameters are combined with the original LLM weights for inference. The PEFT weights are trained for each task and can be easily swapped out for inference, allowing efficient adaptation of the original model to multiple tasks. There are several methods you can use for parameter efficient fine-tuning, each with trade-offs on parameter efficiency, memory efficiency, training speed, model quality, and inference costs. 

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230802200511131.png" alt="image-20230802200511131" style="zoom: 25%;" />

**表格1: 完全微調的挑戰**

| 挑戰       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| 計算需求   | 完全微調需要大量的計算資源來訓練模型                         |
| 記憶體需求 | 需要記憶體來儲存模型和其他訓練過程中的參數                   |
| 儲存問題   | 每個任務的微調都會產生一個新的模型，可能會產生大量的儲存需求 |
| 災難性遺忘 | 完全微調可能導致模型遺忘先前訓練的任務                       |

**表格2: PEFT與完全微調的比較**

| 屬性       | 完全微調                         | PEFT                                           |
| ---------- | -------------------------------- | ---------------------------------------------- |
| 參數更新   | 更新所有模型參數                 | 只更新一部分模型參數                           |
| 記憶體需求 | 高                               | 相對較低                                       |
| 訓練速度   | 慢                               | 相對較快                                       |
| 多任務適應 | 需要對每個任務都訓練一個新的模型 | 可以輕易地在推理中替換訓練的權重以適應多種任務 |

**表格3: PEFT的三大類型**

| 方法類型     | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| 選擇性方法   | 只對原始LLM參數的一部分進行微調                              |
| 再參數化方法 | 使用原始LLM參數，但通過創建原始網絡權重的新的低秩變換，減少了需要訓練的參數數量 |
| 添加法       | 保持所有原始LLM權重的凍結並引入新的可訓練組件                |



## PEFT tech1: LoRA (Low Rank Adaption of LLMs)

**LoRA的工作原理**：

| 步驟                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| 1. 凍結原始模型參數 | LoRA首先將所有的原始模型參數凍結，防止在微調過程中被更新。   |
| 2. 注入秩分解矩陣   | LoRA在原始權重旁注入一對秩分解矩陣，這對較小的矩陣將在後續步驟中進行訓練。 |
| 3. 設定矩陣維度     | 這對較小的矩陣的維度被設定為它們的乘積是一個與它們修改的權重相同維度的矩陣。 |
| 4. 訓練小矩陣       | 在保持原始LLM（大語言模型）的權重凍結的情況下，使用之前見過的監督式學習過程訓練這些較小的矩陣。 |
| 5. 矩陣相乘         | 在推理時，這兩個低秩矩陣被相乘以創建一個與凍結權重相同維度的矩陣。 |
| 6. 更新權重         | 然後將這個新創建的矩陣加到原始權重上，並將模型中的這些原始權重替換為這些更新的值。 |

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230802192846899.png" alt="image-20230802192846899" style="zoom: 20%;" />

| 步驟            | 說明                                                         |
| --------------- | ------------------------------------------------------------ |
| 輸入處理        | 輸入提示轉換為代碼，然後轉換為嵌入向量，並傳遞到Transformer的編碼器和/或解碼器部分。 |
| Transformer組件 | 編碼器和解碼器有兩種類型的神經網路：自我關注和前饋網路。這些網路的權重在預訓練期間學習。 |
| 完全微調        | 在完全微調期間，更新這些層的所有參數。                       |
| LoRA策略        | LoRA通過凍結原始模型的所有參數，然後將一對秩分解矩陣注入到原始權重旁邊，減少微調期間需要訓練的參數數量。 |
| 保持原權重      | 保持LLM的原始權重凍結，並使用之前看到的相同的監督學習過程訓練較小的矩陣。 |
| 推斷            | 在推斷時，將兩個低秩矩陣相乘以創建一個與凍結權重具有相同維度的矩陣。然後將其添加到原始權重並替換模型中的這些更新值。 |
| LoRA微調模型    | 您現在有一個可以執行特定任務的LoRA微調模型。由於該模型的參數數量與原始模型相同，因此對推斷延遲幾乎沒有影響。 |
| 應用LoRA        | 研究人員發現，僅將LoRA應用於模型的自我注意層通常就足夠進行任務的微調並實現性能增益。然而，原則上，您也可以在前饋層等其他組件上使用LoRA。但是，由於LLMs的大部分參數都在注意層中，因此通過將LoRA應用於這些權重矩陣，您可以獲得最大的訓練參數節省。 |



## PEFT tech2: Soft Prompt, also called Prompt Tuning

<img src="https://p.ipic.vip/2tgwuh.png" alt="image-20230802195050954" style="zoom: 25%;" />

<img src="https://p.ipic.vip/nhzm0k.png" alt="image-20230802195125915" style="zoom:25%;" />

<img src="https://p.ipic.vip/633jbo.png" alt="image-20230802195351887" style="zoom: 33%;" />







# Refs

## **Multi-task, instruction fine-tuning**

- [**Scaling Instruction-Finetuned Language Models**](https://arxiv.org/pdf/2210.11416.pdf) - Scaling fine-tuning with a focus on task, model size and chain-of-thought data.
- [**Introducing FLAN: More generalizable Language Models with Instruction Fine-Tuning**](https://ai.googleblog.com/2021/10/introducing-flan-more-generalizable.html) - This blog (and article) explores instruction fine-tuning, which aims to make language models better at performing NLP tasks with zero-shot inference.

## **Model Evaluation Metrics**

- [**HELM - Holistic Evaluation of Language Models**](https://crfm.stanford.edu/helm/latest/) - HELM is a living benchmark to evaluate Language Models more transparently. 
- [**General Language Understanding Evaluation (GLUE) benchmark**](https://openreview.net/pdf?id=rJ4km2R5t7) - This paper introduces GLUE, a benchmark for evaluating models on diverse natural language understanding (NLU) tasks and emphasizing the importance of improved general NLU systems.
- [**SuperGLUE**](https://super.gluebenchmark.com/) - This paper introduces SuperGLUE, a benchmark designed to evaluate the performance of various NLP models on a range of challenging language understanding tasks.
- [**ROUGE: A Package for Automatic Evaluation of Summaries**](https://aclanthology.org/W04-1013.pdf) - This paper introduces and evaluates four different measures (ROUGE-N, ROUGE-L, ROUGE-W, and ROUGE-S) in the ROUGE summarization evaluation package, which assess the quality of summaries by comparing them to ideal human-generated summaries.
- [**Measuring Massive Multitask Language Understanding (MMLU)**](https://arxiv.org/pdf/2009.03300.pdf) - This paper presents a new test to measure multitask accuracy in text models, highlighting the need for substantial improvements in achieving expert-level accuracy and addressing lopsided performance and low accuracy on socially important subjects.
- [**BigBench-Hard - Beyond the Imitation Game: Quantifying and Extrapolating the Capabilities of Language Models**](https://arxiv.org/pdf/2206.04615.pdf) - The paper introduces BIG-bench, a benchmark for evaluating language models on challenging tasks, providing insights on scale, calibration, and social bias.

## **Parameter- efficient fine tuning (PEFT)**

- [**Scaling Down to Scale Up: A Guide to Parameter-Efficient Fine-Tuning**](https://arxiv.org/pdf/2303.15647.pdf) - This paper provides a systematic overview of Parameter-Efficient Fine-tuning (PEFT) Methods in all three categories discussed in the lecture videos.
- [**On the Effectiveness of Parameter-Efficient Fine-Tuning**](https://arxiv.org/pdf/2211.15583.pdf) - The paper analyzes sparse fine-tuning methods for pre-trained models in NLP.

## **LoRA**

- [**LoRA Low-Rank Adaptation of Large Language Models**](https://arxiv.org/pdf/2106.09685.pdf) -  This paper proposes a parameter-efficient fine-tuning method that makes use of low-rank decomposition matrices to reduce the number of trainable parameters needed for fine-tuning language models.
- [**QLoRA: Efficient Finetuning of Quantized LLMs**](https://arxiv.org/pdf/2305.14314.pdf) - This paper introduces an efficient method for fine-tuning large language models on a single GPU, based on quantization, achieving impressive results on benchmark tests.

## **Prompt tuning with soft prompts**

- [**The Power of Scale for Parameter-Efficient Prompt Tuning**](https://arxiv.org/pdf/2104.08691.pdf) - The paper explores "prompt tuning," a method for conditioning language models with learned soft prompts, achieving competitive performance compared to full fine-tuning and enabling model reuse for many tasks.