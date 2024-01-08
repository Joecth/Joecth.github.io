---
layout: post
categories: LLM
tag: []
date: 2023-07-29
Author: Jo
---



# Generative AI w/ LLM



# Intro

## Generative AI w/ LLM

2017 年，谷歌和多倫多大學發表了《Attention is All You Need》這篇論文後，一切都發生了變化。 Transformer 架構已經到來。 這種新穎的方法開啟了我們今天看到的生成人工智能的進步。 

它可以

- 有效地擴展以使用多核 GPU
- 並行處理輸入數據，利用更大的訓練數據集，
- 最重要的是，它能夠學會關注正在處理的單詞的含義。 您所需要的就是關注。 



Ambiguity: Bank

These are `homonyms`. In this case, it's only with the context of the sentence that we can see what kind of `bank` is meant. 



Syntactic ambiguity: whose Book?

Words within a sentence structures can be `ambiguous` or have what we might call `syntactic ambiguity`. Take for example this sentence, "The teacher taught the students with the book." Did the teacher teach using the book or did the student have the book, or was it both?

<img src="https://p.ipic.vip/edg63s.png" alt="image-20230802125204881" style="zoom: 33%;" />



## Transformer Architecture

sequence2sequence + self-attention

<img src="https://p.ipic.vip/jrujlb.png" alt="img" style="zoom: 77%;" />

> "Attention is All You Need" is a research paper published in 2017 by Google researchers, which introduced the Transformer model, a novel architecture that revolutionized the field of natural language processing (NLP) and became the basis for the LLMs we now know - such as GPT, PaLM and others. The paper proposes a neural network architecture that replaces traditional recurrent neural networks (RNNs) and convolutional neural networks (CNNs) with an entirely attention-based mechanism. 
>
> The Transformer model uses self-attention to compute representations of input sequences, which allows it to capture long-term dependencies and parallelize computation effectively. The authors demonstrate that their model achieves state-of-the-art performance on several machine translation tasks and outperform previous models that rely on RNNs or CNNs.
>
> The Transformer architecture consists of an encoder and a decoder, each of which is composed of several layers. Each layer consists of two sub-layers: a multi-head self-attention mechanism and a feed-forward neural network. The multi-head self-attention mechanism allows the model to attend to different parts of the input sequence, while the feed-forward network applies a point-wise fully connected layer to each position separately and identically. 
>
> The Transformer model also uses residual connections and layer normalization to facilitate training and prevent overfitting. In addition, the authors introduce a positional encoding scheme that encodes the position of each token in the input sequence, enabling the model to capture the order of the sequence without the need for recurrent or convolutional operations.



Each token ID in the vocabulary is matched to a multi-dimensional vector, and the <u>intuition is that these vectors learn to encode the meaning and context of individual tokens in the input sequence</u>.



原理概要: 

1. 字被轉為token，有其standard，可能照一個word或是一個word的部份(我們知道"部分"一般有其函義，如er), 在生成時要用一樣的分詞器喲
   - 回顧word2vec paper的概念。然後transformer paper裡用的是512的vector

2. 放入嵌入層，結合位置層的info以保留字間的關係

3.  attention 允許模型關注輸入序列的不同部分，以更好地捕獲單詞之間的上下文依賴關係。 在訓練期間學習並存儲在這些層中的自註意力權重反映了重要性，該輸入序列中每個單詞相對於序列中所有其他單詞的比例
4. 這會有多頭的attention，每個關注可能不同特性，如人物、實體、壓韻等等
5. 最後傳給FC 是個logits 向量，與 tokenizer 字典中每個 token 的概率得分成正比。
6. 然後，您可以將這些 logits 傳遞到最終的 softmax 層，在其中將它們標準化為每個單詞的概率得分。，然後出去給softmax



架構概要:

- "Attention is All You Need" 是由 Google 研究員在 2017 年發表的一篇研究論文，其中引入了 Transformer 模型，這是一種革新性的架構，徹底改變了自然語言處理（NLP）領域，並成為我們現在所知道的如 GPT、PaLM 等大型語言模型 (LLMs) 的基礎。這篇論文提出一種神經網絡架構，用全注意力機制取代傳統的遞歸神經網絡（RNNs）和卷積神經網絡（CNNs）。
- Transformer 模型使用自我注意力來計算輸入序列的表示，這使它能有效地捕捉長期依賴性並並行計算。作者證明他們的模型在多個機器翻譯任務上達到了最先進的性能，並超越了依賴 RNNs 或 CNNs 的先前模型。
- Transformer 架構由編碼器和解碼器組成，每個部分都由多個層組成。每個層都由兩個子層組成：多頭自我注意力機制和前饋神經網絡。多頭自我注意力機制使模型能夠關注輸入序列的不同部分，而前饋網絡則對每個位置分別且相同地應用全連接層。
- Transformer 模型還使用殘差連接和層正規化來促進訓練並防止過度擬合。此外，作者引入了一種位置編碼方案，對輸入序列中每個 token 的位置進行編碼，使模型能夠在無需遞歸或卷積操作的情況下捕捉序列的順序。



<img src="https://p.ipic.vip/9tgj64.png" alt="image-20230801112600856" style="zoom: 25%;" />

<img src="https://p.ipic.vip/ybxbno.png" alt="image-20230801112636111" style="zoom:25%;" />





<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230801113212896.png" alt="image-20230801113212896" style="zoom:25%;" />

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230801113302281.png" alt="image-20230801113302281" style="zoom:25%;" />



<img src="https://p.ipic.vip/pc6kc2.png" alt="image-20230801113514240" style="zoom:25%;" />

<img src="https://p.ipic.vip/hs81dx.png" alt="image-20230801113706004" style="zoom:25%;" />



## Gen text w/ transformer

此時，離開編碼器的數據是輸入序列的結構和含義的深度表示
離開編碼器的數據是輸入序列的結構和含義的深度表示。 該表示被插入到解碼器的中間以影響解碼器的自註意力機制。 接下來，將序列開始標記添加到解碼器的輸入。 這會觸發解碼器預測下一個令牌，它是根據編碼器提供的上下文理解來預測下一個令牌的。 解碼器自註意力層的輸出通過解碼器前饋網絡並通過最終的 softmax 輸出層。 此時，我們有了第一個令牌。



可以通過多種方式使用 softmax 層的輸出來預測下一個標記。 這些會影響生成的文本的創意程度。

![image-20230801120748737](https://p.ipic.vip/4f1c29.png)



## Promp Eng. (In-Context Learning)

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230801145113929.png" alt="image-20230801145113929" style="zoom:15%;" />



## Generative Configuration

<img src="https://p.ipic.vip/nrlcgd.png" alt="image-20230801145459701" style="zoom:25%;" />



| 參數名稱           | 解釋                                                         | 例子                                                         |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **max_new_tokens** | 限制模型生成的最大token數量。這可以被視為對模型選擇過程的最大次數設定一個上限。 | 假設 max_new_tokens 被設定為 100，150，或 200，模型生成的token數將不會超過這些數值。 |
| **sample_top_k**   | 給模型指定只從具有最高概率的 k 個token中選擇。這可以幫助模型有一定的隨機性，同時避免選擇高度不可能的完成詞。 | 如果 k 被設定為 3，模型將只從最可能的三個選項中選擇。        |
| **sample_top_p**   | 限制隨機抽樣只用於總概率不超過 p 的預測。也就是說，所有選擇的概率加起來不會超過 p。 | 如果 p 被設定為 0.3，模型會選擇概率為 0.2 和 0.1 的 token，因為他們的概率和為 0.3。 |
| **temperature**    | 控制模型輸出隨機性的參數。它影響模型計算下一個token的概率分佈的形狀。溫度值越高，隨機性越高；溫度值越低，隨機性越低。 | 假設溫度值低於1，這將會使 softmax 層的概率分佈更為集中，大多數概率集中在少數幾個詞上；如果溫度值設定為高於1，則模型將計算出一個較為平坦的概率分佈，概率會在各個token間更為均勻。 |

> 這四種參數（max_new_tokens，sample_top_k，sample_top_p，以及 temperature）都與 softmax 層的輸出有關，並且主要影響模型在推斷階段生成文本的方式



## Proj Lifecycle

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230801152653222.png" alt="image-20230801152653222" style="zoom: 33%;" />



## LAB1 - FlanT5, prompt eng.

- general model, prompt eng.

Flan-T5

用不到的instructions prompt, 看model是不是有表現得比較好？

one-shot ，用一個完整的例子，包含「這個人類覺得的answer」，然後給第二個例子，讓它作出如summary的answer

not cheating, we're helping the model itself , inexpensive way to try out models! 



- 實驗摘要：

1. 實驗開始於使用Python 3和特定的程式庫進行設定，包括 PyTorch, Torch data 和 Transformers 等。
2. 實驗的目標是用語言模型（在此為FLAN-T5模型）對對話進行摘要，用於比如客服對話等場景。
3. 實驗使用的資料集名為 Dialogue sum，其中包含人與人之間的對話以及對話的人工摘要，作為基線比較。
4. 首先，直接傳遞原始對話給模型進行摘要，結果並不理想，許多細節並未被捕捉到。
5. 為了提高摘要的質量，實驗引入了 "prompt engineering" 的概念，即傳遞特定的指令來改變模型的行為。這包括 "zero-shot inference with an instruction"、 "one-shot learning" 和 "few-shot learning"。雖然有些改善，但效果仍然不是很好。
6. 在 "one-shot learning" 中，給模型提供一個完整的對話範例（包括人工摘要），然後給出第二個需要模型進行摘要的對話。這比zero-shot有些許改善。
7. 在 "few-shot learning" 中，給模型提供多個完整的對話範例（包括人工摘要），然後給出另一個需要模型進行摘要的對話。結果顯示，與one-shot相比，這並沒有太大的改善，顯示增加更多範例可能並不會提高摘要的質量。
8. 最後，實驗將探索如何調整生成模型的參數，如調整溫度以影響模型生成的創新程度。例如，增加溫度可以讓模型生成更有創意的回應，而降低溫度會讓模型的回應更為保守。

- 比較不同的 "shot"：

1. "Zero-shot"：直接傳遞原始對話給模型進行摘要，並沒有給模型任何對話範例。此種情況下，模型並不能很好地摘要對話，很多細節並未被捕捉到。
2. "One-shot"：給模型提供一個完整的對話範例（包括人工摘要），然後給出第二個需要模型進行摘要的對話。此種情況下，模型的摘要有所改善。
3. "Few-shot"：給模型提供多個完整的對話範例（包括人工摘要），然後給出另一個需要模型進行摘要的對話。結果顯示，與one-shot相比，這並沒有太大的改善，顯示增加更多範例可能並不會提高摘要的質量。



# LLM Pretraining & Scaling laws

## Pretraining LLMs

Model Hubs

![image-20230801224427672](https://p.ipic.vip/9ueya9.png)



LLMs encode a deep statistical representation of language. 

1. LLM 的初始訓練過程通常被稱為預訓練階段。在此階段，模型從大量的非結構化文本數據中學習，以編碼深度統計語言表示。這個學習過程需要大量的計算資源和 GPU。

2. 預訓練階段的目標是最小化訓練目標的損失。在此過程中，模型權重會被更新，編碼器會為每個令牌生成一個嵌入或向量表示。

3. 有三種 Transformer 模型的變體：只有編碼器的模型、只有解碼器的模型和包含編碼器和解碼器的模型。每一種都有不同的訓練目標，因此學習執行不同的任務。

4. - 只有編碼器的模型（如BERT和RoBERTa）是使用遮蔽語言建模進行預訓練的，適合從雙向上下文中受益的任務，例如情感分析或命名實體識別。
   - 只有解碼器的模型（如GBT和BLOOM）是使用因果語言建模進行預訓練的，常用於文本生成。這些模型通過預測下一個令牌來建立語言的統計表示。
   - 序列到序列模型（如T5和BART）同時使用編碼器和解碼器進行預訓練，適用於翻譯、摘要和問答等任務。T5 的全名是 "Text-to-Text Transfer Transformer"。這是一個通用的大型語言模型，由 Google Research 的團隊開發

5. 儘管更大的模型通常更能有效地完成任務，但訓練這些巨大的模型非常困難且成本高昂，因此不斷訓練更大和更大的模型可能是不可行的。對此，下一段視頻將進一步探討訓練大型模型所面臨的挑戰



### Encode-only

<img src="https://p.ipic.vip/dc5zn5.png" alt="image-20230801230449054" style="zoom: 33%;" />





### Decode-only

<img src="https://p.ipic.vip/pcrzmg.png" alt="image-20230801230817344" style="zoom:33%;" />



### Encode+Decode, sequence2sequence

<img src="https://p.ipic.vip/2iq460.png" alt="image-20230801231356176" style="zoom: 33%;" />



### Summary!

<img src="https://p.ipic.vip/bmhcxf.png" alt="image-20230801231840733" style="zoom:33%;" />

| 模型類型                         | 訓練目標                                                     | 應用場景                                                     | 是否有利於大模型 | 代表模型      |
| -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------- | ------------- |
| Encoder-only (自編碼模型)        | 使用蒙版語言建模，隨機遮蔽輸入序列中的 tokens，並試圖預測遮蔽的 tokens | 句子分類任務（例如情感分析），或 token 級任務（例如命名實體識別或單詞分類） | 一般             | BERT, RoBERTa |
| Decoder-only (自回歸模型)        | 使用因果語言建模，基於前面的一系列 tokens 來預測下一個 token | 文本生成；對於大型模型，展示出強大的零樣本推理能力，並能很好地完成各種任務 | 對於大模型有利   | GPT, BLOOM    |
| Encoder-Decoder (序列對序列模型) | 使用跨度腐敗（Span Corruption）進行訓練，隨機遮蔽輸入 token 的序列 | 翻譯、摘要、問答                                             | 一般             | T5, BART      |

請注意，儘管大型的 Decoder-only 模型在應用場景上更為廣泛，但是訓練這些模型的資源需求、訓練難度和成本也相對較高。

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230801232210879.png" alt="image-20230801232210879" style="zoom: 25%;" />



## Computational Challenges of training LLMs

> OutOfMemoryError: CUDA out of memory..!



<img src="https://p.ipic.vip/qgeop8.png" alt="image-20230802013617666" style="zoom:25%;" />

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230802013643585.png" alt="image-20230802013643585" style="zoom:25%;" />



<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230802013853436.png" alt="image-20230802013853436" style="zoom:25%;" />





| 數據類型                 | bits | 指數 | 分數 | 記憶體需求 |
| ------------------------ | ---- | ---- | ---- | ---------- |
| FP32                     | 32   | 8    | 23   | 4 bytes    |
| FP16                     | 16   | 5    | 10   | 2 bytes    |
| BFLOAT16, *FLAN-T5用這個 | 16   | 8    | 7    | 2 bytes    |
| INT8                     | 8    | N/A  | N/A  | 1 byte     |

*注意：對於INT8，並不會有指數和分數的表示，因為它是一個整數類型。

此外，這裡也是一些重要的摘要點：

1. 量化的目標是通過降低模型權重的精度來減少存儲和訓練模型所需的記憶體。
2. 量化將原始的32位浮點數統計投影到使用基於原始32位浮點數範圍計算的縮放因子的更低精度空間。
3. 現代的深度學習框架和庫支持量化感知訓練 Quantization-aware training(QAT)，這在訓練過程中學習量化縮放因子。
4. 量化可以用來減少訓練過程中模型的記憶體占用。
5. BFLOAT16已經成為深度學習中的一種熱門精度選擇，因為它保持了FP32的動態範圍，但將記憶體占用減半。
6. 許多大型語言模型（LLMs），包括FLAN-T5，都已經用BFLOAT16預訓練過。



存1Billion的FP32需要4GB的RAM



<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230802014842420.png" alt="image-20230802014842420" style="zoom: 25%;" />



## Training Across GPUs

### DDP，(Distributed Data Parallel)

![image-20230802112224890](https://p.ipic.vip/1c4szn.png)





### ZeRO, (Zero Redundancy Optimizer)

ZeRO 是由 Microsoft 的深度速度團隊開發的一種技術，該技術主要針對模型參數、優化器狀態和梯度進行優化，以減少 GPU 記憶體的使用並提高訓練大型模型的效率。

以下是一些關於 ZeRO 的更多信息：

1. **模型參數、優化器狀態和梯度的分片**：ZeRO 的核心思想是分片模型參數、優化器狀態和梯度。這意味著這些元素被均勻地分散在所有可用的 GPU 中，而不是在每個 GPU 上存儲完整的副本。這消除了冗餘的記憶體使用，並允許在 GPU 記憶體有限的情況下訓練更大的模型。
2. **優化器狀態分片（ZeRO-1）**：這是減少記憶體使用量的第一步，優化器狀態通常比模型參數需要更多的記憶體，因此分片它們可以節省大量記憶體。
3. **梯度分片（ZeRO-2）**：當應用 ZeRO-1 的同時，也將梯度分片，可以進一步減少記憶體使用量。
4. **模型參數分片（ZeRO-3）**：此階段將模型參數分片，進一步節省記憶體。這使得模型的規模可以超過單個 GPU 的記憶體限制。
5. **通訊優化**：ZeRO 還包括一種被稱為 "重疊通訊與計算" 的技術，這種技術可以在計算過程中進行數據的交換，從而減少等待時間並提高效率。
6. **兼容性**：ZeRO 與 PyTorch 和 TensorFlow 等深度學習框架都相容。具體實現包括 DeepSpeed（一個由微軟開發的深度學習優化庫）和 FairScale（Facebook 的一個開源專案）。
7. **與其他分散式訓練策略的組合**：ZeRO 可以與 Data Parallelism 和 Model Parallelism 等其他分散式訓練策略結合使用，以進一步提高訓練大型模型的效率。例如，微軟的 DeepSpeed 團隊提出了一種稱為 "ZeRO-Offload" 的技術，該技術將一部分的優化器狀態和梯度計算卸載到 CPU，進一步減少 GPU 記憶體的使用並允許在有限的硬體資源下訓練更大的模型。

這些特性使 ZeRO 成為一種非常強大的工具，尤其是對於訓練大型深度學習模型的場景，它可以有效地解決記憶體限制問題。



### FSDP, (Fully Sharded Data Parallelism) 

1. **FSDP 概述**：FSDP (Fully Sharded Data Parallelism) 是一種分散式訓練技術，它結合了數據平行訓練（如 DDP）與 ZeRO 的模型狀態分片技術。當使用 FSDP 時，不僅數據分佈在多個 GPU 中，模型參數、梯度和優化器狀態也跨 GPU 節點進行分片。
2. **操作過程**：與 DDP 不同，FSDP 在執行前向和反向傳播時需要收集所有 GPU 的數據。每個 CPU 都會按需從其他 GPU 獲取數據，將分片的數據臨時轉換為未分片的數據以進行操作。操作完成後，將非本地的未分片數據釋放回其他 GPU，或者可以選擇在後續操作（例如反向傳播）中保留它。
3. **梯度同步**：反向傳播完成後，FSDP 會以與 DDP 相同的方式跨 GPU 同步梯度。
4. **模型分片**：FSDP 允許進行模型分片，從而減少總體的 GPU 記憶體使用。你還可以選擇讓 FSDP 把部分訓練計算卸載到 GPU，以進一步減少 GPU 記憶體使用。
5. **分片級別的配置**：你可以使用 FSDP 的分片因子來管理性能和記憶體使用之間的權衡。分片因子為1就意味著去除分片並複製完整模型，這與 DDP 類似。如果將分片因子設置為最大的 GPU 數量，則開啟完全分片，這樣可以節省最多的記憶體，但會增加 GPU 之間的通訊量。設定在這兩者之間的任何分片因子則可啟用超級分片。
6. **FSDP vs DDP 性能比較**：根據實驗數據，當模型大小超過22.8億參數時，DDP 會遇到記憶體不足的問題，而 FSDP 則可以輕鬆處理這種大小的模型，並在降低模型精度至16位時達到更高的 teraflops。然而，當模型在越來越多的 GPU 上分佈時，通訊量的增加會開始影響性能，使計算變慢。
7. **適用場景**：你可以在小型和大型模型上使用 FSDP，並無縫地將模型訓練擴展到多個 GPU。然而，由於跨 GPU 訓練模型的成本和技術複雜性，一些研究者正在尋找能夠用更小模型達到更好性能的方法。

：

|                                                              |                     FSDP - Full Sharding                     | FSDP - Hyper Sharding | FSDP - Full Replication |             DDP             |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :-------------------: | :---------------------: | :-------------------------: |
|           **611 Million Parameters (Model 1-25)**            |                  Similar Performance to DDP                  |     Not Provided      |      Not Provided       | Similar Performance to FSDP |
|           **2.28 Billion Parameters (Model 1-25)**           |                  Similar Performance to DDP                  |     Not Provided      |      Not Provided       | Similar Performance to FSDP |
|            **11.3 Billion Parameters (Model 25)**            | Can Handle and Achieve Higher Teraflops with 16-bit Precision |     Not Provided      |      Not Provided       |     Out-of-Memory Error     |
| **Impact of Increasing GPU from 8 to 512 (11 Billion T5 Model)** |               7% Decrease in per GPU Teraflops               |     Not Provided      |      Not Provided       |        Not Provided         |

> - FSDP - Full Replication 跟	DDP 意義上有什麼差別呀？如下
>
>   | Aspect                    | FSDP - Full Replication                                      | DDP                                      |
>   | ------------------------- | ------------------------------------------------------------ | ---------------------------------------- |
>   | Data Parallelism          | Yes                                                          | Yes                                      |
>   | Model Replication         | Yes                                                          | Yes                                      |
>   | Memory Management         | More efficient (due to model sharding)                       | Standard (full model on each GPU)        |
>   | Sharding Control          | Flexible (allows trade-off between performance and memory usage) | No flexibility                           |
>   | Suitable for Large Models | More suitable (due to model sharding and flexible control)   | Less suitable for extremely large models |





### Scaling Laws & Compute-Optimal Models

每秒一千萬萬次的浮點運算（petaFLOP/s）相當於八個NVIDIA V100 GPU滿載運行一整天，或者兩個NVIDIA A100 GPU滿載運行一整天。

根據這個比例，我們可以計算出下表：

| Model | Parameter Quantity | Compute Budget (petaFLOP/s-days) | Equivalent No. of NVIDIA V100 GPUs (1 day) | Equivalent No. of NVIDIA A100 GPUs (1 day) |
| ----- | ------------------ | -------------------------------- | ------------------------------------------ | ------------------------------------------ |
| T5 XL | 3 billion          | 100                              | 800                                        | 200                                        |
| GPT-3 | 175 billion        | 3700                             | 29600                                      | 7400                                       |

需要注意的是，這裡的計算僅僅是個概略的估算，真實情況可能會因為其他因素（例如具體的訓練實現、軟體優化等）而有所不同。

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230802121714590.png" alt="image-20230802121714590" style="zoom:33%;" />

![image-20230802122024515](https://p.ipic.vip/ysdk4x.png)



 Jordan Hoffmann, Sebastian Borgeaud 和 Arthur Mensch 在 2022 年發表的研究。該研究的目標是找出給定計算預算下的最佳參數數量和訓練數據量，並產生了一個被稱為 "Chinchilla" 的計算最優模型。以下是文章的主要摘要和比較：

1. **過度參數化與訓練不足**：許多具有 1000 億參數的大型語言模型（如 GPT-3）可能實際上是過度參數化的，這意味著它們的參數數量超過了獲得良好語言理解的需要，並且訓練不足，對更多的訓練數據有利。作者假設，如果在更大的數據集上訓練，較小的模型可能能夠達到與大型模型相同的性能。
2. **訓練數據集的最佳大小**：Chinchilla 的一個重要發現是，給定模型的最佳訓練數據集大小約為模型參數數量的 20 倍。例如，對於一個有 70 億參數的模型，最佳的訓練數據集包含 1.4 兆個 tokens。
3. **Chinchilla 模型的優勢**：在大範圍的下游評價任務上，計算最優的 Chinchilla 模型優於非計算最優的模型，如 GPT-3。有了 Chinchilla 研究的結果，團隊已經開始開發相似或更好的模型，而這些模型比以非最優方式訓練的大型模型更小。
4. **模型設計的優化趨勢**：未來可以期待看到更多團隊或開發者像你一樣開始優化他們的模型設計，從而偏離過去幾年的 "越大越好" 趨勢。
5. **Bloomberg GPT 模型**：最後一個顯示在這張幻燈片上的 Bloomberg GPT 模型是一個很有趣的模型。該模型按照 Chinchilla 的方式進行了計算最優的訓練，因此，儘管參數數量為 500 億，但仍實現了良好的性能。此外，該模型也是一個從頭開始訓練模型以實現良好任務性能的有趣例子。



## Pretraining for Domain Adaptation

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230802124542725.png" alt="image-20230802124542725" style="zoom: 33%;" />

#### BloombergGPT

BloombergGPT 是一種只有解碼器的大型語言模型，特別是為金融領域做預訓練。Bloomberg 的研究人員選擇結合金融數據和通用稅務數據進行預訓練，該模型在金融基準測試上獲得最佳成績，同時在通用語言模型基準測試上也表現競爭力。因此，研究人員選擇的數據由51%的金融數據和49%的公共數據組成。

在訓練 BloombergGPT 的過程中，作者使用了 Chinchilla 縮放規則來指導模型中的參數數量和訓練數據的體積（以 token 衡量）。Chinchilla 的建議在圖像中由 Chinchilla-1、Chinchilla-2 和 Chinchilla-3 的線條表示，我們可以看到 BloombergGPT 跟它們很接近。

雖然為團隊可用的訓練計算預算推薦的配置是 500 億個參數和 1.4 兆個 token，但在金融領域獲取 1.4 兆個 token 的訓練數據證明具有挑戰性。因此，他們構建了一個只包含 7000 億個 token 的數據集，少於計算最佳值。此外，由於早期停止，訓練過程在處理了 5690 億個 token 後結束。

BloombergGPT 項目是一個很好的例子，說明了增加模型特定領域性的預訓練，以及可能迫使模型和訓練配置作出與計算最佳模型相對的權衡的挑戰。



## Q&A

Which of the following statements about pretraining scaling laws are correct? Select all that apply:



- To scale our model, we need to jointly increase dataset size and model size, or they can become a bottleneck for each other.
  - Incorrect

- There is a relationship between model size (in number of parameters) and the optimal number of tokens to train the model with.

  - Correct

    This relationship is describe in the Chinchilla paper, that shows that many models might even be overparametrized according to the relationship they found.

- When measuring compute budget, we can use "PetaFlops per second-Day" as a metric.

  - Correct

    Petaflops per second-day is a useful measure for computing budget as it reflects the both hardware and time required to train the model.

- You should always follow the recommended number of tokens, based on the chinchilla laws, to train your model.
  - This should not be selected
    - Although compute optimization is important, it can be challenging to obtain a sufficient amount of data tokens. In the case of BloombergGPT, they had a limited token count and even used fewer tokens due to early stopping. While chinchilla laws offer valuable guidance, they should not be strictly followed as rigid rules.





# HW

What is the *self-attention* that powers the transformer architecture?

1 point



v A mechanism that allows a model to focus on different parts of the input sequence during computation.

