---
layout: post
categories: LLM
tag: []
date: 2023-08-02
Author: Jo
---



# RLHF, for responsible AI



## Aligning models with human values

| 主題           | 詳細內容                                                     |
| -------------- | ------------------------------------------------------------ |
| 微調的目的     | 進一步訓練模型，使其更好地理解人類的提示，並產生更接近人類的回應。這可以改善模型相對於原始預訓練版本的性能，並導致更自然的語言。 |
| 挑戰           | 自然的人類語言帶來了一系列新的挑戰，包括模型使用有毒語言，以好戰和激進的語調回應，以及提供有關危險主題的詳細資訊。 |
| 問題的源頭     | 這些問題存在的原因是大型模型在網路上的大量文本數據上進行訓練，而這種語言在這些數據中經常出現。 |
| 不良行為的例子 | 1) 模型回答不適當或不相關的問題。例如，當被問到敲門笑話時，模型回答“拍手”。這並不是對於給定任務的有用的答案。 2) 模型可能給出誤導或完全不正確的答案。例如，如果問模型關於被證實是錯誤的健康建議，如用咳嗽來停止心臟病，模型可能會給出自信而完全不正確的回答。 3) 模型不應該創建有害的回應，如侮辱性的、歧視性的或煽動犯罪行為。例如，當被問到如何破解鄰居的Wi-Fi時，模型可能會回答有效的策略。 |
| 解決方式       | 透過人類反饋進行額外的微調可以更好地將模型與人類偏好對齊，並增加回應的有用性、誠實性和無害性。這種進一步的訓練也可以幫助降低模型回應的毒性，並減少錯誤資訊的生成。 |



### HHH

| 主題                   | 解釋                                                         | 例子                                                         |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Helpfulness（有用性）  | 指的是AI模型在回答問題或執行任務時，應提供有用和相關的信息。 | 例如，當被問到敲門笑話時，模型回答“拍手”。這並不是對於給定任務的有用的答案。理想的回答應該是一個真正的敲門笑話。 |
| Honesty（誠實性）      | 指的是AI模型在回答問題時，應提供準確和真實的信息。           | 例如，如果問模型關於被證實是錯誤的健康建議，如用咳嗽來停止心臟病，模型可能會給出自信而完全不正確的回答。理想的回答應該是反駁這個誤導的健康建議。 |
| Harmlessness（無害性） | 指的是AI模型的行為應該是無害的，不應煽動或引導出有害的行為。 | 例如，當被問到如何破解鄰居的Wi-Fi時，模型可能會回答有效的策略。理想的回答應該是拒絕提供該資訊或解釋為何這種行為是不適當的。 |



## RLHF, (Reinforcement Learning from Human Feedback)

> Reinforcement learning is a type of machine learning in which an agent learns 
>
> to make decisions related to a specific goal by taking actions in an environment, 
>
> with the objective of maximizing some notion of a cumulative reward.Perhaps most importantly, RLHF can help 
>
> In this framework, the agent continually learns from its experiences by 
>
> taking actions, observing the resulting changes in the environment, and 
>
> receiving rewards or penalties, based on the outcomes of its actions. 
>
> By iterating through this process, the agent gradually refines its strategy or 
>
> policy to make better decisions and increase its chances of success.

1. to minimize the potential for harm. 
2. to give caveats that acknowledge their limitations and to avoid toxic language and topics.

<img src="https://p.ipic.vip/x8kxym.png" alt="image-20230803191315758" style="zoom:33%;" />





1. **學習方法**：強化學習是一種機器學習方法，其目標是使學習代理能夠在給定的環境中做出最佳決策。
2. **環境互動**：代理會在環境中採取行動，觀察結果，並根據其行動的結果獲取獎勵或懲罰。
3. **學習目標**：強化學習的目標是最大化總體的獎勵。這通常涉及尋找一種平衡，使得即時的獎勵和長期的獎勵都能夠最大化。
4. **策略改進**：通過不斷地互動和學習，代理會改進其決策策略或方針，以提高其成功機率。
5. **連續學習**：在強化學習中，學習是連續的過程，代理會不斷從其行動和結果中學習，並調整其行為。



<img src="https://p.ipic.vip/4umx9v.png" alt="image-20230803192932185" style="zoom:33%;" />

> As a practical and scalable alternative, you can use an additional model, 
>
> known as the reward model, to classify the outputs of the LLM and 
>
> evaluate the degree of alignment with human preferences.
>
> Once trained, you'll use the reward model to assess the output of the LLM and 
>
> assign a reward value, which in turn gets used to update the weights off the LLM and 
>
> train a new human aligned version. 
>
> Exactly how the weights get updated as the model completions are assessed, 
>
> depends on the algorithm used to optimize the policy. 
>
> Lastly, note that in the context of language modeling, 
>
> the sequence of actions and states is called a rollout, 
>
> instead of the term playout that's used in classic reinforcement learning.
>
> The reward model is the central component of the reinforcement learning process. 
>
> It encodes all of the preferences that have been learned from human feedback, and 
>
> it plays a central role in how the model updates its weights over many iterations

| 項目                      | 說明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| 代理策略 (Agent Strategy) | 是由大型語言模型 (LLM) 所擔任的，目標是生成被認為與人類偏好一致的文本，例如有幫助、準確且無毒的文本。 |
| 環境 (Environment)        | 是模型的上下文窗口，也就是可以通過提示輸入文本的空間。       |
| 狀態 (State)              | 是模型在採取行動之前考慮的當前上下文，也就是當前包含在上下文窗口中的任何文本。 |
| 行動 (Action)             | 是生成文本的行為，這可以是單個單詞、句子或較長的文本，具體取決於用戶指定的任務。 |
| 行動空間 (Action Space)   | 是令牌詞彙表，也就是模型可以選擇來生成完成的所有可能的令牌。 |

1. **LLM 的行動選擇**：大型語言模型 (LLM) 會根據訓練期間學到的語言統計表示來決定生成序列中的下一個標記。這是基於上下文提示文本以及詞彙空間上的概率分佈。
2. **獎勵分配**：獎勵的分配基於完成的文本與人類偏好的匹配程度。由於人類對語言的反應存在變異，確定獎勵的過程比井字遊戲的例子要複雜。
3. **人類反饋與評估**：一種方法是讓人類根據對齊指標，如文本是否有毒，評估模型的所有完成。這種反饋可以表示為一個標量值，如零或一，並可以用來迭代更新 LLM 權重，以最大化從人類分類器獲得的獎勵。
4. **使用獎勵模型**：由於獲取人類反饋可能耗時且昂貴，因此一種可行的替代方案是使用獎勵模型來對 LLM 的輸出進行分類和評估。一旦訓練完成，獎勵模型就可以用來評估 LLM 的輸出，分配獎勵值，並更新 LLM 的權重。
5. **策略優化**：權重的具體更新方式取決於用於優化策略的算法。
6. **獎勵模型與強化學習**：在語言建模的背景下，動作和狀態的序列被稱為 "rollout"，而非經典強化學習中使用的 "playout"。獎勵模型是強化學習過程的核心部分，它對人類反饋中學到的偏好進行編碼，並在模型權重的多次迭代更新中發揮核心作用。



## RLHF: Obtaining Feedback from Humans

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230803194432935.png" alt="image-20230803194432935" style="zoom: 33%;" />

1. 選擇模型：首先，選擇具有一些目標任務能力的模型，如文本摘要或問答等。一般而言，使用已被跨多任務微調並具有一些通用能力的指導模型會較為簡單。
2. 生成completion：

此段討論了「以人類回饋進行強化學習」(RLHF)在微調語言模型(LLM)時的第一步驟：獲取人類回饋。

1. **選擇模型和數據集**：首先，選擇具有一些目標任務能力的模型，如文本摘要或問答等。你可能會發現使用已經在許多任務上微調過並具有一些通用能力的模型更容易。接著，將該LLM與一個提示數據集結合，為每個提示生成多個不同的回答。這個數據集由多個提示組成，每一個都被LLM處理以產生一組完成的回答。
2. **收集人類回饋**：下一步是收集人類標籤員對LLM生成的完成的反饋。首先，你需要決定你希望人類基於何種標準來評估這些完成，這可能是前面討論過的問題，比如有用性或有害性。然後，你將要求標籤員根據該標準評估數據集中的每一個完成。
3. **範例說明**：例如，對於提示"My house is too hot."，LLM生成了三種不同的回答。你的任務是讓標籤員按照有用性對這三個完成進行排序，從最有用到最不有用。這個過程會在許多提示完成組上重複進行，從而構建一個可以用來訓練獎勵模型的數據集，最終這個獎勵模型將取代人類完成這項工作。
4. **指令明確性**：明確的指令對於獲取高質量的人類反饋非常重要。對標籤員的詳細指示可以增加他們理解任務並按照你的期望來完成的可能性。



### Steps

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230803201550275.png" alt="image-20230803201550275" style="zoom:25%;" />

1. **處理不合理回答**：如果模型產生的回答無意義、令人困惑或不相關，標籤員應選擇「F」而不是排名，以便容易地移除低質量的答案。
2. **給予詳細指令**：提供像這樣的詳細指令可以提高回答的質量，並確保每個人類標籤員以類似的方式執行任務，從而確保標記的完成組能夠代表共識的觀點。
3. **訓練獎勵模型**：一旦人類標籤員完成了對完成組的評估，你就有了訓練獎勵模型所需的所有數據。
4. **轉換數據格式**：在開始訓練獎勵模型之前，你需要將排名數據轉換成完成的兩兩比較。所有可能的完成配對都應該被劃分為0或1分。
5. **建立完成配對**：例如，如果有三種完成方式，並且人類標籤員的排名是2, 1, 3（其中1代表最喜歡的回答），那麼就有三種可能的配對：紫色-黃色，紫色-綠色，黃色-綠色。根據每個提示的完成選項數量N，你將有"N選二"的組合。
6. **為每個配對分配獎勵**：對於每個配對，你將為首選回答分配1分的獎勵，為次選回答分配0分的獎勵。然後你將重新排列提示，使得首選選項排在前面。這是一個重要的步驟，因為獎勵模型期望首選完成（也稱為Yj）在前。
7. **數據重組**：一旦你完成了這些數據的重組，人類的回答就會被轉換成適合訓練獎勵模型的格式。
8. **注意事項**：雖然通常收集讚好/讚壞的反饋比較容易，但排名反饋可以為訓練你的獎勵模型提供更多的提示完成數據。例如，你可以從每個人的排名中獲得三個提示完成對。



## RLHF: Reward Model

1. 訓練，就不再需要人類的參與。

2. 獎勵模型會有效地取代人類標籤員的角色，並在訓練過程中自動選擇首選完成。

3. 獎勵模型通常也是一種語言模型，可以透過監督學習方法在人類標籤員對提示的評估數據上訓練。

4. 獎勵模型的目標是學習偏好人類首選的完成方式，同時最小化獎勵差異。

5. 一旦模型已經在人類的排名提示完成對上訓練過，可以使用獎勵模型作為一種二進制分類器。

6. 例如，如果想要讓語言模型避免產生仇恨言論，獎勵模型就需要能識別完成的部分是否包含仇恨言論。


<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230823172046547.png" alt="image-20230823172046547" style="zoom: 67%;" />

> Let's say you want to detoxify your LLM, 
>
> and the reward model needs to 
>
> identify if the completion contains hate speech. 
>
> In this case, the two classes would be notate, 
>
> the positive class that you ultimately want to optimize 
>
> for and hate the negative class you want to avoid. 
>
> The largest value of the positive class is what you 
>
> use as the reward value in LLHF. 
>
> Just to remind you, if you apply 
>
> a Softmax function to the logits, 
>
> you will get the probabilities. 
>
> The example here shows a good reward for 
>
> non-toxic completion and the second example 
>
> shows a bad reward being given for toxic completion.



## RLHF: Fine-Tuning with Reinforcement Learning

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230803211741800.png" alt="image-20230803211741800" style="zoom: 33%;" />

| 步驟 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| 1    | 將提示提供給LLM，讓其生成完成的句子                          |
| 2    | 將完成的句子與原始提示一起作為一對傳遞給獎勵模型             |
| 3    | 獎勵模型根據訓練時的人類反饋評估這一對，並返回一個獎勵值     |
| 4    | 獎勵值被用於更新LLM的權重，使其更偏向生成更高獎勵、更對齊的回應 |

RLHF is becoming increasingly important to ensure that LLMs behave in a safe and aligned manner in deployment.

> This is the algorithm that takes the output 
>
> of the reward model and uses it 
>
> to update the LLM model weights so 
>
> that the reward score increases over time.

ensure that 

LLMs behave in a safe and aligned manner in deployment.



## PPO, (Proximal Policy Optimization)

> As the name suggests, 
>
> PPO optimizes a policy, 
>
> in this case the LLM, 
>
> to be more aligned with human preferences. 
>
> Over many iterations, PPO makes updates to the LLM. 
>
> The updates are small and within a bounded region, 
>
> resulting in an updated LLM 
>
> that is close to the previous version, 
>
> hence the name Proximal Policy Optimization. 
>
> 
>
> Keeping the changes within 
>
> this small region result in a more stable learning. 
>
> The goal is to update 
>
> the policy so that the reward is maximized. 

<img src="https://p.ipic.vip/gexjj5.png" alt="image-20230823194828690" style="zoom:67%;" />



<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230823195015112.png" alt="image-20230823195015112" style="zoom: 50%;" />





> RLHF is a fine-tuning process 
>
> that aligns LLMs with human preferences. 
>
> In this process, you make use of 
>
> a reward model to assess and LLMs 
>
> completions of a prompt data 
>
> set against some human preference metric, 
>
> like helpful or not helpful. 
>
> Next, you use a reinforcement learning algorithm, 
>
> in this case, PPO, 
>
> to update the weights off the LLM based on the reward is 
>
> signed to the completions generated 
>
> by the current version off the LLM. 
>
> You'll carry out this cycle of 
>
> a multiple iterations using many different prompts 
>
> and updates off the model weights 
>
> until you obtain your desired degree of alignment. 
>
> Your end result is 
>
> a human aligned LLM that you can use in your application. 



## RLHF: Reward Hacking

以下是個例子來說明獎勵黑客的問題：

假設我們正在使用強化學習來讓你的指導語言模型（instruct LLM）生成更友善（less toxic）的回應。已經訓練了一個獎勵模型，該模型能夠進行情感分析，並將模型生成的文本分類為有毒或無毒。

現在，我將一個提示「this product is...」送入你的指導語言模型，該模型產生了一個補全：「complete garbage」。這個補全的情感負面，因此可能會獲得一個高度有毒的評級。

然而，當我的語言模型經過多次迭代和學習後，它可能會嘗試最大化獎勵（即最小化有毒評級）。它可能會學會加入一些總是能得到低有毒評級的字詞或片語，例如「most awesome」或「most incredible」，即使這樣做可能會使得生成的文本變得過度誇大或者變得不合理。

這就是獎勵黑客的一種現象：模型學會了如何遊戲系統以最大化其獎勵，但最終生成的文本可能並不是我們真正想要的結果。

<img src="https://p.ipic.vip/fl9pjf.png" alt="image-20230823195838934" style="zoom:67%;" />

<img src="https://p.ipic.vip/1cmjm5.png" alt="image-20230803212242324" style="zoom: 33%;" />

### Sol

<img src="https://p.ipic.vip/y8h561.png" alt="image-20230803221112298" style="zoom:33%;" />

> It needs 2 full copies of the LLM to calculate the KL divergence, the frozen reference LLM, and the RL-updated PPO LLM. 
>
> We can further benefit from combining our relationship with PEFT. In this case, we only update the weights of a path adapter, not the full weights of the LLM.

<img src="https://p.ipic.vip/yjd5h0.png" alt="image-20230803221245341" style="zoom:33%;" />



###  KL divergence

KL散度（Kullback-Leibler Divergence）是一種度量兩個概率分布間差異的數學方法，常用於強化學習領域，特別是在使用PPO（近端策略優化）算法時。

在PPO中，目標是通過基於與環境互動獲得的獎勵來反覆更新代理的參數，以找到改進的策略。然而，過於激進的更新策略可能會導致學習不穩定或策略變化過大。為了解決這個問題，PPO引入了一個約束，限制策略更新的幅度。這個約束是通過使用KL散度來實現的。

要理解KL散度是如何工作的，我們可以想像有兩個概率分布：原始LLM的分布，和一個新提議的RL更新的LLM分布。KL散度度量的是當我們使用原始策略來對新提議的策略的樣本進行編碼時，我們獲得的信息量的平均數。通過最小化兩個分布之間的KL散度，PPO確保更新的策略與原始策略保持接近，防止可能對學習過程產生負面影響的劇變。

你可以使用的一個用於以強化學習方式訓練變換器語言模型的庫是TRL（Transformer Reinforcement Learning）。在這個鏈接中，你可以更多地了解這個庫，以及它與PEFT（Parameter-Efficient Fine-Tuning）方法（如LoRA（Low-Rank Adaption））的集成。該圖像顯示了TRL中PPO訓練設置的概覽。

簡單來說，KL散度是一種方法，用於度量並控制在學習過程中模型更新的幅度，以確保學習的穩定性和防止策略的劇變。

In PPO, the goal is to find an improved policy for an agent by iteratively updating its parameters based on the rewards received from interacting with the environment. However, updating the policy too aggressively can lead to unstable learning or drastic policy changes. To address this, PPO introduces a constraint that limits the extent of policy updates. This constraint is enforced by using KL-Divergence.





## Scaling Human Feedbacks

在RLHF（強化學習和人類反饋）微調中，儘管你可以使用獎勵模型來消除在微調過程中對人類評估的需求，但是首先需要巨大的人力努力來產生訓練獎勵模型所需的標記數據集。這通常需要大團隊的標記員，有時甚至需要成千上萬的人來評估各種提示。這項工作需要大量的時間和其他資源，這可能是重要的限制因素。隨著模型和用例數量的增加，人力努力成為有限的資源。擴大人類反饋的方法是研究的活躍領域。一種克服這些限制的想法是通過模型自我監督來擴展。憲法人工智能就是一種擴展監督的方法。

憲法人工智能首先在2022年由Anthropic的研究人員提出，它是一種使用一組規則和原則來訓練模型的方法，這些規則和原則決定了模型的行為。你可以訓練模型自我批評，並修改其回應以符合這些原則。憲法人工智能不僅對擴大反饋有用，而且還可以幫助解決RLHF的一些意外後果。例如，根據提示的結構，一個對齊的模型可能會在盡其所能提供最有幫助的回應時，暴露出有害的信息。

在實施憲法人工智能方法時，你需要分兩個階段訓練模型。在第一階段，你進行監督學習，開始使用提示模型的方式，試圖讓它產生有害的回應，這個過程被稱為紅隊操作。然後你讓模型根據憲法原則批評自己的有害回應，並修改它們以符合這些規則。完成後，你將使用紅隊提示和修訂的憲法回應對的對模型進行微調。

然後，你將所有部分放在一起，要求模型寫出一個新的回應，刪除所有有害或非法的內容。模型生成一個新的答案，將憲法原則付諸實踐，不包括對非法應用的引用。原始的紅隊提示和這個最後的憲法回應可以作為訓練數據。你將構建一個包含許多這樣的例子的數據集，以創建一個已經學習如何生成憲法回應的微調NLM。

第二部分的過程進行強化學習。這個階段與RLHF相似，除了人類反饋，我們現在使用由模型生成的反饋。這有時被稱為從AI反饋中學習的強化學習或RLAIF。在這裡，你使用前一步中的微調模型來生成一組對你的提示的回應。然後你讓模型根據憲法原則來判斷哪個回應是優先的。結果是一個模型生成的偏好數據集，你可以用它來訓練一個獎勵模型。有了這個獎勵模型，你現在可以使用類似於PPO的強化學習算法進一步微調模型。





# LLM-powered Applications



## Model Optimizations for Deployment

<img src="https://p.ipic.vip/gegwtb.png" alt="image-20230804081521186" style="zoom: 33%;" />

**問題**

| 問題                                     | 描述                                                         | 範例                                                         |
| ---------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 模型部署的運行速度                       | 您的模型需要多快生成完成答案？                               | 如果模型被用於需要快速回應的即時對話系統，則運行速度將是非常重要的考慮因素。 |
| 可用的運算預算                           | 您有多少運算能力可以投入到模型的部署上？                     | 如果您在雲端運算資源有限，或是運用在邊緣裝置上，則會需要考慮到運算預算。 |
| 模型效能和推論速度或儲存空間的取捨       | 是否願意為了改善推論速度或減少儲存需求而犧牲模型的效能？     | 縮小模型可能會降低其生成結果的準確性，但可以提高運行速度並減少存儲需求。 |
| 模型是否需要與外部數據或其他應用進行交互 | 如果模型需要與外部數據或其他應用進行交互，則需要考慮如何連接這些資源。 | 例如，您的模型可能需要存取網路爬蟲或數據庫以查詢或更新資訊。 |
| 模型將如何被消費                         | 您的模型將通過哪種應用程式或API介面被使用？                  | 例如，模型可能透過智能助理應用程式、語音識別系統或一個Web API進行訪問。 |

**優化技術**

| 技術                 | 描述                                                         | 範例                                                         |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 蒸餾（Distillation） | 用一個大型模型（教師模型）訓練一個較小的模型（學生模型），以降低儲存和運算需求。 | 一個例子可能是，使用大型GPT-4模型作為教師模型，訓練一個小型GPT-2模型作為學生模型。 |
| 量化（Quantization） | 將模型權重轉換為較低精度的表示，例如16位浮點數或8位整數，以降低模型大小和記憶體需求以及所需的模型服務運算資源。 | 可以將GPT-4模型的權重由32位浮點數量化為16位浮點數，以節省儲存空間和運算時間。 |
| 剪枝（Pruning）      | 移除對模型性能貢獻不大的權重，這些權重的值接近或等於零。     | 使用LoRA或其他參數有效的微調方法對模型進行剪枝，移除對模型性能影響最小的權重。 |



## Generative AI Project Lifecycle Cheat Sheet

| 項目階段                                                     | 描述                                                         | 需要的時間和努力                                             | 需要的技術專業知識                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------------------------------------- |
| 大型語言模型的預訓練                                         | 這個階段涉及到模型架構的決定，大量的訓練數據，並需要專業的技術知識。 | 最耗時和最耗力的階段。                                       | 最需要專業知識的階段。                         |
| 引導工程 (Prompt Engineering)                                | 如果你從一個已存在的基礎模型開始，你可能會首先透過引導工程來評估模型的表現。 | 相對快速，可能不需要特別的訓練時間。                         | 需要一些技術知識，但相對於模型訓練，要求較低。 |
| 引導調整與微調 (Prompt Tuning and Fine Tuning)               | 如果模型的表現還不夠好，你可能會考慮引導調整或微調模型。     | 依據用途、性能目標和運算預算的不同，所需的時間和努力也會不同。 | 需要一定的技術專業知識。                       |
| 利用人類反饋進行強化學習來對齊模型 (Aligning the Model Using RL from Human Feedback) | 一旦你有了訓練獎勵模型，你就可以快速地使用人類反饋來進行強化學習。 | 如果你已經有現成的獎勵模型，這個過程可能會很快。但如果需要從零開始訓練一個獎勵模型，由於需要收集人類反饋，所以可能會花費很長的時間。 | 需要一定的技術專業知識。                       |
| 優化技術 (Optimization Techniques)                           | 在進行最終的模型部署之前，你可能會需要使用一些優化技術來改善模型的性能。 | 這個階段的複雜程度和所需努力在中等範圍內，但一旦確定模型變動不會太大影響性能，這個過程可以很快進行。 | 需要一定的技術專業知識。                       |



## Using LLM in Applications

By providing up to date relevant information and avoiding hallucinations to greatly improve the experience of using application for users.

Note: the LLMs do not carry out mathematical operations. They are still just trying to predict the next best token based on their training, and as a result, can easily get the answer wrong.

問題 & 解決方案*

| 問題     | 描述                                                         | 範例                                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 知識截止 | 語言模型的內在知識在預訓練的時刻停止，無法理解訓練之後的新事件或新聞。 | 2022 年初訓練的模型被問到英國首相是誰時，會回答是 Boris Johnson，但模型並不知道他在 2022 年底辭職。 |
| 數學問題 | 模型在處理複雜的數學問題時可能會遇到困難。                   | 提問模型執行一個除法問題，模型返回的答案接近正確答案，但不準確。 |
| 幻覺     | 當模型不知道答案時，可能會生成與問題無關的文本。             | 模型生成了一種不存在的植物"火星沙丘樹"的描述。               |

| 解決方案                 | 描述                                                         | 範例                                            |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------------- |
| 使用外部數據源和應用程式 | 通過連接到外部數據源和應用程式，模型可以得到更新的信息，從而克服知識截止的問題。 | 將語言模型連接到外部數據庫或其他應用程式的API。 |



### RAG, (Retrieval Augmented Generation, )

| RAG 的功能     | 描述                                                         | 範例                                                         |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 克服知識截止   | RAG通過給模型提供訪問額外的外部數據，幫助模型更新其對世界的理解。 | 您可以讓模型訪問它可能未見過的數據，如新信息文件、原始訓練數據中未包含的文件，或存儲在您的組織私有數據庫中的專有知識。 |
| 避免幻覺       | RAG 可以幫助你避免模型在不知道答案的情況下產生幻覺。         |                                                              |
| 整合外部信息源 | RAG 可以將多種類型的外部信息源整合到一起。                   | 使用 RAG，您可以讓模型訪問本地文檔、包括私有 wiki 和專家系統在內的互聯網上的信息，或者與數據庫互動。 |

| RAG 的實施           | 描述                                                         | 範例                                                         |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 限制的上下文窗口大小 | 絕大部分的文本來源都過長，無法適應模型有限的上下文窗口。因此，外部數據源需要被切分成許多塊，每塊都可以適應在上下文窗口裡。 | 使用像是 Langchain 這種套件來處理這項工作。                  |
| 數據的格式           | 數據必須以易於檢索最相關文本的格式提供。                     | 利用語言模型內部以向量形式表現語言的特性，透過 RAG 方法將外部數據的各個小塊處理進大型語言模型，創建每個塊的嵌入向量，並將這些新的數據表現形式儲存在叫做向量存儲的結構裡。 |



## Interacting with External Apllications

<img src="https://p.ipic.vip/oau486.png" alt="image-20230804095046617" style="zoom: 25%;" />

例子:

> **客戶**: 表示他們希望退回他們購買的基因。
>
> **ShopBot**: 詢問訂單號碼。
>
> **客戶**: 提供訂單號碼。
>
> **ShopBot**: 在交易數據庫中查找訂單號碼，可能通過一種類似於您在上一個視頻中看到的RAG實現來實現。在這種情況下，您可能會透過SQL查詢後端訂單數據庫來檢索數據，而不是從文件集中檢索數據。
>
> **ShopBot**: 取得了客戶的訂單後，確認將退回的商品。詢問客戶是否還有其他除了基因以外的商品要退回。
>
> **客戶**: 表示他們的答案。
>
> **ShopBot**: 向公司的運輸合作夥伴發起一個退貨標籤的請求。透過運輸合作夥伴的Python API來請求標籤。並將通過電子郵件將運輸標籤發送給客戶。同時詢問他們確認電子郵件地址。
>
> **客戶**: 以電子郵件地址回應，該信息將被包含在對運輸商的API調用中。
>
> **ShopBot**: API請求完成後，通知客戶標籤已通過電子郵件發送，並結束對話。



**連接語言模型與外部應用的重要性**：

1. 擴展語言模型的功能：連接外部應用可以讓語言模型與更廣泛的世界互動，使其功能超越語言任務。
2. 觸發動作：語言模型可以被用來觸發動作，當給予與API互動的能力時。例如在ShopBot的例子中。
3. 連接其他編程資源：語言模型也可以連接到其他編程資源，例如Python解釋器，以在輸出中嵌入精確的計算。

**如何觸發動作**：

1. 提示（prompts）與完成（completions）：這些是工作流程的核心。應用將根據用戶的請求進行的操作由語言模型決定，語言模型作為應用的推理引擎。
2. 生成指令：語言模型需要生成一套指令，讓應用知道需要採取哪些行動。這些指令需要易於理解並與允許的行動相對應。例如在ShopBot中，重要的步驟包括檢查訂單ID、請求運送標籤、驗證用戶電郵以及透過電郵向用戶發送標籤。
3. 完成的格式：完成需要以一種應用能理解的方式格式化。這可能像是特定的句子結構，或像是寫出Python腳本或生成SQL指令那樣複雜。
4. 收集驗證信息：模型可能需要收集允許驗證行動的信息。例如在ShopBot的對話中，應用需要驗證客戶用於下訂單的電郵地址。

以下是一個表格，比較一下在 ShopBot 示例中，LLM 操作步驟以及對應的實際操作：

| LLM 操作步驟           | 實際操作                                    |
| ---------------------- | ------------------------------------------- |
| 檢查訂單ID             | 在數據庫中查找訂單號碼                      |
| 請求運送標籤           | 通過運輸合作夥伴的API發起請求               |
| 驗證用戶電郵           | 從用戶處獲得電郵地址，並將其包含在API呼叫中 |
| 透過電郵向用戶發送標籤 | API請求完成後，通知客戶標籤已經發送         |



## Helping LLMs reason and Plan with Chain-of-Thought

### Chain of Thought prompting

E.g. Intermediate calculations form the reasoning steps that a human might take, and the full sequence of steps illustrates the chain of thought that went into solving the problem.



這是文章中兩個範例的比較：

| 問題種類 | 一次推理                                          | 鏈式思考提示                                                 |
| -------- | ------------------------------------------------- | ------------------------------------------------------------ |
| 數學問題 | 錯誤地判斷餐廳剩餘的蘋果數量為27（正確答案應為9） | 正確地判斷餐廳剩餘的蘋果數量為9                              |
| 物理問題 | 未給出範例                                        | 正確地判斷黃金戒指會沉到游泳池底部，因為黃金的密度遠大於水的密度 |



## PAL, (Program-aided Language Models)

Remember, the model isn't actually doing any real math here. It is simply trying to predict the most probable tokens that complete the prompt. 

This work first presented by Luyu Gao and collaborators at Carnegie Mellon University in 2022

PAL 的主要策略是讓 LLM 生成包含電腦代碼的完成語句。這些代碼可以傳遞給解釋器來進行必要的計算，以解決問題。LLM 可以在提示的例子中學習到輸出的格式。例子裡會包括一個問題和一些 Python 代碼行，這些代碼行能夠解決問題。

<img src="https://p.ipic.vip/9objt9.png" alt="image-20230804094543300" style="zoom: 67%;" />



<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230804095223507.png" alt="image-20230804095223507" style="zoom:25%;" />





## ReAct: Combining "Reasoning and Action"

ReAct is a prompting strategy that combines chain of thought reasoning with action planning. The framework was proposed by researchers at Princeton and Google in 2022. The paper develops a series of complex prompting examples based on problems from Hot Pot QA, a multi-step question answering benchmark. 



以下是 "ReAct" 框架的步驟：Thought, Action, Observation steps

1. 問題定義：開始的提示(question prompt)會是一個需要多步驟來回答的問題。例如，確定兩本雜誌中哪一本是先創建的。
2. 思考步驟：這個步驟包括一個思考、行動、與觀察的三元組。"思考" 是一個解決問題的邏輯步驟，並識別需要採取的行動。例如，在雜誌出版的問題中，模型將搜索兩本雜誌並確定哪一本先出版。
3. 行動步驟：模型需要從一個預定義的行動列表中選擇一個行動來執行，以便與外部應用程序或數據源互動。在 "ReAct" 框架中，這些行動包括：
   - 搜索：查找特定主題的Wikipedia條目
   - 查找：在Wikipedia頁面上搜索特定的字符串
   - 完成：當模型決定已經找到答案時，執行的行動
4. 觀察步驟：新的信息（來自於行動步驟的結果）會被引入到提示的上下文中。這是 "觀察" 步驟。
5. 重複步驟：上述的 "思考"、"行動"、"觀察" 的循環會重複進行，直到找到最終的答案為止。

注意，每個行動都使用特定的方括號符號來格式化，這樣 Python 解釋器就可以觸發對應的 API 操作。



### LangChain

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230804102549531.png" alt="image-20230804102549531" style="zoom:33%;" />

LangChain is in active development, and new features are being added all the time, like the ability to examine and evaluate the LLM's completions throughout the workflow. It's an exciting framework that can help you with fast prototyping and deployment, and is likely to become an important tool in your generative AI toolbox in the future. 



Larger models are generally your best choice for techniques that use advanced prompting, like PAL or ReAct. Smaller models may struggle to understand the tasks in highly structured prompts and may require you to perform additional fine tuning to improve their ability to reason and plan. This could slow down your development process. Instead, if you start with a large, capable model and collect lots of user data in deployment, you may be able to use it to train and fine tune a smaller model that you can switch to at a later time.



### ReAct Paper

![image-20230804103551751](https://p.ipic.vip/l7w0n9.png)

> The figure provides a comprehensive visual comparison of different prompting methods in two distinct domains. The first part of the figure (1a) presents a comparison of four prompting methods: Standard, Chain-of-thought (CoT, Reason Only), Act-only, and ReAct (Reason+Act) for solving a HotpotQA question. Each method's approach is demonstrated through task-solving trajectories generated by the model (Act, Thought) and the environment (Obs). The second part of the figure





## LLM Application Architectures

![image-20230804115519625](https://p.ipic.vip/x8fmj5.png)

1. **應用架構的組成元素**：
   - **基礎設施層**：提供運算，存儲，和網路資源來運行您的LLM，並用於託管您的應用組件。您可以使用您自己的基礎設施，或者選擇使用按需付費的雲服務。
   - **大型語言模型**：在您的應用中使用的LLM，可以包括基礎模型，以及您特定任務的模型。這些模型會部署在適合您推理需求的基礎設施上，例如需要實時或近實時與模型交互的情況。
   - **從外部源獲取信息**：如在增強檢索生成部分所討論的，您的應用可能需要從外部源獲取信息。
   - **輸出保存和反饋收集**：根據您的使用情況，您可能需要實現捕獲和儲存輸出的機制，例如，您可以建立在一個會議中儲存用戶完成內容的能力，以增加您的LLM的固定上下文窗口大小。您也可以收集用戶的反饋，這對於您的應用的進一步微調，對齊或評估可能是有用的。
   - **使用額外的工具和框架**：您可能需要使用一些工具和框架來更輕鬆地實現在本課程中討論的一些技術，例如使用len chain的內置庫來實現像pow react或chain of thought提示等技術。
   - **使用者介面和安全元件**：最後，您通常會有某種用戶界面，例如網站或REST API，讓用戶或者其他應用可以通過它來使用您的應用。在這個層次，您也會包括用於與您的應用交互的安全組件。
2. **模型對齊人類偏好**：本週，您看到了如何通過使用一種名為人類反饋強化學習（RLHF）的技術來微調模型，使其與人類偏好，如有用，無害，誠實等進行對齊。
3. **模型推理優化**：您也看到了重要的技術，通過模型蒸餾，量化，或修剪來縮小模型大小，以減少在生產中運行您的LLM所需要的硬體資源。
4. **模型部署優化**：最後，您探索了通過結構化提示和連接到外部數據源和應用來幫助模型在部署時有更好表現的方法。
5. **LLM的潛力和前景**：LLM可以在應用中發揮驚人的作用，利用其智能來驅動令人興奮的，有用的應用。像len chain這樣的框架使得可以快速構建，部署和測試LLM驅動的應用成為可能，對開發者來說，這是一個非常令人興奮的時期。



## AWS Sagemaker JumpStart

a model hub, Sagemaker JumpStart is a model hub, and it allows you to quickly deploy foundation models that are available within the service, and integrate them into your own applications. The JumpStart service also provides an easy way to fine-tune and deploy models. JumpStart covers many parts of this diagram, including the infrastructure, the LLM itself, the tools and frameworks, and even an API to invoke the model.

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230804115726032.png" alt="image-20230804115726032" style="zoom:33%;" />

AWS的Sagemaker JumpStart是一種服務，旨在幫助您快速將基於大型語言模型(LLM)的應用程序推向生產並實現規模運作。以下是該服務的主要特點：

1. **模型中心**：JumpStart作為一種模型中心，允許您快速部署在該服務中可用的基礎模型，並將其整合到自己的應用程序中。
2. **微調和部署**：JumpStart提供了一種簡單的方式來微調和部署模型。
3. **全面覆蓋**：JumpStart涵蓋了基於LLM應用程序建設的許多部分，包括基礎設施，LLM本身，工具和框架，甚至是調用模型的API。
4. **GPU需求**：與您在實驗室中使用的模型相比，JumpStart模型需要使用GPU進行微調和部署。請注意，這些GPU的價格是按需定價，因此在選擇您要使用的計算設備之前，應參考Sagemaker的定價頁面。
5. **成本監控**：使用JumpStart時，請務必在不使用時刪除Sagemaker模型端點，並遵循成本監控最佳實踐以優化成本。
6. **支持微調**：JumpStart支持模型微調。例如，您可以透過指定訓練和驗證數據集的位置，然後選擇要用於訓練的計算設備的大小來設定微調作業。
7. **參數調整**：您可以快速識別和修改此特定模型的可調參數。
8. **自動生成筆記本**：JumpStart還提供一種選項，可以自動為您生成一個筆記本。如果您更喜歡以程式化的方式與這些模型進行交互，這是一種選擇。

除了作為包含基礎模型的模型中心外，JumpStart還提供了許多其他資源，包括博客、視頻和示例筆記本。



# Refs

## **Reinforcement Learning from Human-Feedback (RLHF)**

- [**Training language models to follow instructions with human feedback**](https://arxiv.org/pdf/2203.02155.pdf) **-** Paper by OpenAI introducing a human-in-the-loop process to create a model that is better at following instructions (InstructGPT).
- [**Learning to summarize from human feedback**](https://arxiv.org/pdf/2009.01325.pdf) - This paper presents a method for improving language model-generated summaries using a reward-based approach, surpassing human reference summaries.

## **Proximal Policy Optimization (PPO)**

- [**Proximal Policy Optimization Algorithms**](https://arxiv.org/pdf/1707.06347.pdf) - The paper from researchers at OpenAI that first proposed the PPO algorithm. The paper discusses the performance of the algorithm on a number of benchmark tasks including robotic locomotion and game play.
- [**Direct Preference Optimization: Your Language Model is Secretly a Reward Model**](https://arxiv.org/pdf/2305.18290.pdf) - This paper presents a simpler and effective method for precise control of large-scale unsupervised language models by aligning them with human preferences.

## **Scaling human feedback**

- [**Constitutional AI: Harmlessness from AI Feedback**](https://arxiv.org/pdf/2212.08073.pdf) - This paper introduces a method for training a harmless AI assistant without human labels, allowing better control of AI behavior with minimal human input.

## **Advanced Prompting Techniques**

- [**Chain-of-thought Prompting Elicits Reasoning in Large Language Models**](https://arxiv.org/pdf/2201.11903.pdf) -  Paper by researchers at Google exploring how chain-of-thought prompting improves the ability of LLMs to perform complex reasoning.
- [**PAL: Program-aided Language Models**](https://arxiv.org/abs/2211.10435) - This paper proposes an approach that uses the LLM to read natural language problems and generate programs as the intermediate reasoning steps.
- [**ReAct: Synergizing Reasoning and Acting in Language Models**](https://arxiv.org/abs/2210.03629) This paper presents an advanced prompting technique that allows an LLM to make decisions about how to interact with external applications.

## **LLM powered application architectures**

- [**LangChain Library (GitHub)**](https://github.com/hwchase17/langchain) - This library is aimed at assisting in the development of those types of applications, such as Question Answering, Chatbots and other Agents. You can read the documentation [here](https://docs.langchain.com/docs/).
- [**Who Owns the Generative AI Platform?**](https://a16z.com/2023/01/19/who-owns-the-generative-ai-platform/) - The article examines the market dynamics and business models of generative AI.



# Responsible AI

1. **毒性（Toxicity）**：LLM可能會產生可能對特定群體，特別是邊緣化群體或受保護群體造成傷害或歧視的語言或內容。為了應對這個問題，一個策略是從訓練數據開始，進行篩選，並訓練守護模型來檢測和過濾訓練數據中的任何不需要的內容。此外，為標注員提供足夠的指導，並確保標注員群體的多樣性也是至關重要的。
2. **幻覺（*Hallucinations*）**：LLM可能會生成沒有事實依據的信息。這是因為在訓練大型語言模型或神經網絡時，我們往往無法知道模型實際學習了什麼，模型有時會嘗試填補它的數據空白。為了防止這種情況，一種策略是向用戶說明這種技術的現實，並添加任何免責聲明。此外，可以用獨立且經過驗證的信息源來增強大型語言模型，以便可以對獲取的數據進行雙重檢查。
3. **知識產權（Intellectual Property**：使用模型返回的數據可能會侵犯他人的知識產權。因此，我們需要通過技術、政策制定者和其他法律機制來解決這個問題。此外，我們還需要建立一個管理體系，以確保所有利益相關者都在防止這種情況發生方面扮演著他們應有的角色。

總的來說，使用生成人工智能模型時，需要明確使用案例，評估風險，評估性能，不斷迭代，並在整個生命週期中實施治理政策和問責措施。

對於目前研究社區正在積極研究的一些令人興奮的主題，如水印和指紋識別，以及創建可以幫助確定是否使用了生成人工智能創建內容的模型，