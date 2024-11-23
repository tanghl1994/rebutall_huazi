# To R1
We sincerely appreciate your constructive feedback of our paper, and we will make certain updates w.r.t. your concerns in our revised version, especially more discussion of Induction Heads as it's a fundamental basis of our algorithm design. 

Q1: Comparison with SnapKV:
We agree that SnapKV is a competitive approach for KV cache compression, especially when user queries are known before generation. Below, we present the results of SnapKV in both query-aware and query-agnostic settings:
	•	Query-aware: SnapKV demonstrates impressive compression ratios and accuracy under known query scenarios.
	•	Query-agnostic: In scenarios where the user query is not pre-defined, SnapKV’s performance deteriorates significantly, as shown by the following results:
xxx
In contrast, RazorAttention maintains robust performance regardless of query availability. This resilience is due to its training-free design and headwise sparse pattern, which ensure minimal information loss across various query types. 
We believe this discussion is crucial and will incorporate it into the revised version to emphasize the practical advantages of RazorAttention over query-dependent methods like SnapKV.

Q2: Detailed setting of experiments and more long-context benchmarks:
We acknowledge the variation in Needle in a Haystack accuracy (approximately 2%) under different prompts. We will clarify the specific prompt used for RazorAttention and all baselines in our revised version.
Meanwhile, Here we inlcude the full results of RazorAttention on Longbench
xxx
As you suggest, we have included the results of xxx below:
xxx.

Q3: The efficiency evaluation:
xxx

Q4: Comparison with [1] and [2]
We respectfully emphasize that RazorAttention is the first method to utilize a headwise sparse pattern for KV cache compression. Below, we summarize the key differences between RazorAttention and the recent works [1] and [2]:
	1.	Comparison with [2]:
In [2], the authors rely on a query-based importance metric to select the “important” tokens (Section 3.3 in their paper). While this approach can perform well when the query is known, it introduces the risk of information loss for future queries, just like SnapKV. 
	2.	Comparison with [1]:
Unlike [1], which employs continual training and knowledge distillation to recover model performance after compression, RazorAttention achieves a nearly lossless compression without any additional training. This training-free design is a more challenging and practical approach. We believe the compensation token we introduced and our head selection method (inductive + echo heads) provide valuable insights into the underlying functionality of LLMs in long-context scenarios.
	3.	Regarding Figure 13(1) in [2]:
In [2] authors did not provide specific details about this experiment. We think the discrepancy might be due to their exclusion of echo heads, which we have found to play a critical role in our results. We are confident in the reproducibility of our results.

# To R2
We sincerely appreciate your recognition of the significance of our work, especially in addressing a pertinent and impactful problem in large language model (LLM) optimization. Below are our responses to your concerns:
Q1: Clarify the experimental setup for H2O.:
For H2O, we ensured a fair comparison by setting the KV cache budget to match that of RazorAttention for each sentence.  We appreciate your suggestion and will clarify this in our revised version.

Q2: The efficiency evaluation:
xxx

Q3: Comparison with previous work [1]:
We acknowledge the similarities with [1] and highlight the following key distinctions:
	1.	Broader Definition of Retrieval Heads:
In [1], the authors define retrieval heads as only the induction heads. In contrast, our analysis shows that beyond the inductive heads, about 1% of echo heads play an equally essential role in retaining model performance (refer to Figure 5 in our paper). Using the narrower definition in [1] results in an approximate 10% accuracy drop in the Needle In A Haystack benchmark, emphasizing the importance of our algorithm design.
	2.	Compensation Token Design:
 Directly applying a truncation on the non-retrieval heads' KV cache introduces a significant accuracy drop (>30%). Our compensation token design mitigates this issue effectively and ensures minimal information loss.
In summary, while [1] provides foundational insights into retrieval heads, our work extends these findings to achieve lossless KV cache compression. We will add a detailed discussion of these distinctions in the revised version.

Q4: Elaborate on the “Retrieve and Process” hypothesis.
The “Retrieve and Process” hypothesis originates from the observed behavior of attention heads in LLMs. Most attention heads focus primarily on recent tokens, while only a few retrieval heads attend to remote tokens to extract relevant information. This mirrors the human cognitive process: recalling pertinent information before generating a response. We will clarify this point in our revised version.

Q5: What does "Protect" mean:
In line 234, “protect” refers to preserving the KV cache in selected heads while discarding remote tokens in others. 

# To R3
Q1: More experimental results:
xxx

Q2: The exact compression ratio:
xxx

Q3: Novelty of RazorAttention compared to related work:
Actually we are the first work that applies the induction heads for KV cache compression, and we introduce two essential strategies beyond the use of induction heads to achieve lossless performance. The concept of induction heads was initially proposed in [1], where the authors observed that specific attention heads exhibit a retrieval pattern by attending to previously existing tokens. Recent work [2] extended this observation to long-context scenarios, demonstrating through ablation studies that induction heads are more critical than other heads under such conditions.
Our work builds on these insights and contributes two major advancements:
	1.	Redefining Retrieval Heads:
Unlike [2], which considers only induction heads as retrieval heads, our analysis identifies that echo heads are equally crucial for preserving performance. For example, when tested on the Needle in a Haystack dataset, using the definition of retrieval heads from [2] leads to a ~10% accuracy drop. By expanding the definition to include copy heads, we ensure comprehensive retention of the model’s capabilities (as shown in Figure 5 of our paper).
	2.	Designing the Compensation Token Strategy:
Directly discarding remote tokens in non-retrieval heads results in significant performance degradation, with accuracy drops exceeding 30%. To address this, we introduce a novel compensation token strategy that condenses the information from discarded tokens. This innovation preserves essential information, mitigating the adverse effects of token removal and enabling efficient KV cache compression without compromising performance.

Q4: Writing Errors
Thank you for pointing out the textual and linguistic issues. We will revise the manuscript thoroughly to address these errors and improve overall clarity. 

# To R4
Q1: The efficiency evaluation:
xxx

Q2: Overhead Analysis
xxx

Q3: More comparison
xxx
Noticed that our algorithm can be perfectly combined with current token-dropping algorithms, such as H2O, SnapKV, MInference. Since we only constrain the cache budget in the short heads while in long heads the KV cache can be further compressed using these algorithms.

Q4: Results of RazorAttention under different compression ratios:
Actually the reason why we only consider one specific ratio is that we found the ratio of the necessary retrieval heads is about 15%. Using fewer retrieval heads would introduce certain accuracy drop (see Table-4 in our paper) while using more heads gives no more accuracy gain. Therefore we only report the comparison result under this compression ratio for all baselines. We will clarify this in our revised version.

Q5: Breakdown of the distribution between retrieval and non-retrieval heads:
According to your suggestion, we plot the distribution of the retrieval and non-retrieval heads in Figurexx for different models. We notice a clear trend that most retrieval heads distribute in the middle depth of the network.


# To R5
Q1: Novelty of RazorAttention:
Our work is the first to apply induction heads for KV cache compression, and we introduce two essential strategies that go beyond the use of induction heads to achieve lossless performance:
	1.	Expanded Definition of Retrieval Heads:
The concept of induction heads was initially introduced in [1], where these heads were identified as attention heads that follow a token-retrieval pattern (attending to previously seen tokens). In [2], the authors extended this observation to long-context scenarios, showing that induction heads are more critical than other heads under such conditions. However, these works considered only induction heads as retrieval heads. Our analysis reveals that echo heads are equally crucial for retaining model performance. For instance, when tested on the Needle in a Haystack dataset, using the definition of retrieval heads from [1] leads to an approximate 10% accuracy drop. By redefining retrieval heads to include both induction and copy heads, we retain the full performance of the model (see Figure 5 in our paper).
	2.	Compensation Token Strategy:
Directly discarding remote tokens in non-retrieval heads results in severe performance degradation, with accuracy drops exceeding 30%. To address this, we designed the compensation token strategy, which condenses the dropped information from these heads into a compact form. This strategy ensures that essential information is preserved while enabling efficient KV cache compression.

Q2: Analysis of the worse cases:
We appreciate your observation regarding the benchmarks where RazorAttention does not achieve the best performance.
From our analysis, we found that most of the challenging cases occur in summarization tasks. The performance loss in these tasks can be attributed to the model’s tendency to produce shorter answers when using RazorAttention. Since summarization tasks often rely on metrics like F1 score, which heavily penalize shorter responses, this behavior leads to lower scores.


Q3: What is the compression ratio for GQA in Llama3-8B-Instruct model? What is the compression ratio for 1024K sequence length?

Q4: Different combinations of the echo heads and induction heads:
Yes, the performance of RazorAttention under different heads budget can be found in Figure.5 and Table.4 of our paper. Basically we noticed that including only 1% of the echo heads can essentially improve the accuracy while we need about 15% induction heads to fully recover the performance of the original model.

# To R6
Q1: Comparison with [1]:
Although [1] does provide a similar finding as ours, directly retaining the KV cache in induction heads introduces significant accuracy drop for tasks such as Needle In A Haystack. We introduce two essential strategies that go beyond the use of induction heads to achieve lossless performance:
	1.	Expanded Definition of Retrieval Heads:
The concept of induction heads was initially introduced in [1], where these heads were identified as attention heads that follow a token-retrieval pattern (attending to previously seen tokens). In [2], the authors extended this observation to long-context scenarios, showing that induction heads are more critical than other heads under such conditions. However, these works considered only induction heads as retrieval heads. Our analysis reveals that echo heads are equally crucial for retaining model performance. For instance, when tested on the Needle in a Haystack dataset, using the definition of retrieval heads from [1] leads to an approximate 10% accuracy drop. By redefining retrieval heads to include both induction and copy heads, we retain the full performance of the model (see Figure 5 in our paper).
	2.	Compensation Token Strategy:
Directly discarding remote tokens in non-retrieval heads results in severe performance degradation, with accuracy drops exceeding 30%. To address this, we designed the compensation token strategy, which condenses the dropped information from these heads into a compact form. This strategy ensures that essential information is preserved while enabling efficient KV cache compression.

Q2: Comparison with [2]:
An obvious advantage of the training-free algorithm is that it can be used as a plug-and-play component for advanced LLM serving system, such as TRT-LLM and Triton, and training-free lossless compression is usually a much more challenging task compared to the training-based algorithms. Moreover, we believe the compensation token we introduced and our head selection method (inductive + echo heads) provide valuable insights into the underlying functionality of LLMs in long-context scenarios.

Q3: How to quickly determine the suitable proportion of Induction Heads and Echo Heads for different models:
Actually we find that for most open-source models, about 15%~20% induction heads+1% echo heads shall be enough without training. However, if there still exists certain performance loss, our receipe is to increase about 3% of the induction heads at one step. We will provide a discussion section about this in our revised version.

Q4: General theoretical analysis explaining why retrieval heads are widely present in various models:
We believe this is a quite important and challenging topic, and we will leave this for future study.

# To R7
Q1: Whether the retrieval pattern holds in general query settings
Thank you for pointing this out. The retrieval patterns remain highly consistent across different input queries, suggesting that they are indeed model-based rather than query-specific. In Figure XXX, we illustrate the retrieval heads selected using various “Needle in a Haystack” queries, demonstrating that the selected heads are unchanged across different inputs. Furthermore, we validate these findings on diverse and complex datasets such as LongBench (Table 3) and InfiniBench/Ruler (results provided below). These benchmarks encompass a wide variety of complex task sources, and RazorAttention successfully retains the original performance. This consistency reinforces the generalizability of the observed retrieval patterns, particularly for lengthy inputs (noting that KV cache compression is not applied to inputs shorter than 4k, as described in Table 2).

Q2: Comparison with SnapKV
xxx

Q3: The retrieval pattern among different queries
As mentioned in response to Q1, the retrieval patterns are highly consistent across different input queries, indicating they are primarily model-based and not query-specific. 

Q4: The effectiveness of the compensation token
In Figure 7, we present ablation studies examining the impact of compensation tokens. Both experiments were conducted with identical KV cache allocations, and the inclusion of compensation tokens resulted in significant accuracy improvements. This evidence highlights the critical role of compensation tokens in mitigating information loss due to KV cache reduction. Exploring the combination of compensation tokens with other baseline algorithms is an exciting direction, and we plan to investigate this in future studies.

Q5: The KV cache budget for different algorithms
Table 2 outlines the KV cache compression setup for RazorAttention. For a fair comparison, the total compression ratio for each baseline was adjusted to match RazorAttention’s configuration, ensuring consistent evaluation across varying input lengths. However, as the input length increases, the absolute KV cache budget may differ proportionally due to RazorAttention’s dynamic compression strategy.

