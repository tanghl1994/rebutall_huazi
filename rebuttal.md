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

Q4: Comparison with [1] and [2]:
We politely point out we are the first work that utilizes a headwise sparse pattern for KV cache compression. Actually in [2] authors still utilize a query-based importance metric for select the *important* tokens (in Section 3.3), which inevitably brings the risk of information loss for future queries. Therefore only [1] and RazorAttention are the algorithms that is feasible under general cases.  The difference between RazorAttention and [1] is summarized as follows:
1) RazorAttention is training-free: In [1] authors use continual training and knowledge distillation to recover the models' performance. However, RazorAttention successfully achieves a nearly lossless compression without any training, which is a more challenging task. We believe the compensation token we included essentially reveals some pattern about how the LLM functions under long-context scenario.
2) Retrieval heads selection: We also noticed that in [1] authors claimed that the selected heads using retrieval patterns is much worse than using training-based algorithm. However, they did not provide the specific details about this experiment. We are pretty confident about the reproductivity of our results. One possible reason of result in Figure.13(1) might be that authors did not include the echo heads (we only use top-1% echo heads) as suggested in our paper.
Although we haven't figure out why we need a few echo heads included and how the compensation token works, we believe RazorAttention indeed shed some lights about the inside functionality of LLM and we will leave this for our future study.

4: Comparison with [1] and [2]
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
In [1], the authors define retrieval heads as only the induction heads. In contrast, our analysis shows that copy heads play an equally essential role in retaining model performance (refer to Figure 5 in our paper). Using the narrower definition in [1] results in an approximate 10% accuracy drop in the Needle In A Haystack benchmark, emphasizing the importance of our broader definition.
	2.	Compensation Token Design:
While [1] does not address the performance degradation caused by discarding tokens in non-retrieval heads, our compensation token design mitigates this issue effectively. This approach ensures minimal information loss, avoiding the significant 30% accuracy drop observed without this compensation.
In summary, while [1] provides foundational insights into retrieval heads, our work builds upon and extends these findings to achieve lossless KV cache compression. We will add a detailed discussion of these distinctions in the revised version to contextualize our contributions.

Yes, the concurrent work [1] also found that several "retrieval heads" contribute most to the long inputs, and there are two major contributions of our work:
1) The definition of the retrieval head is different: In [1], authors only considered the induction heads as the retrieval heads. Our analysis reveals that the copy heads are also very essential
for retaining all the performance (see Figure 5 in our paper). Using the retrieval heads defined in [1] results in about 10% of accuracy drop when tested using Needle In A Haystack.
2) The design of the compensation token: Directly discarding all the remote tokens in the non-retrieval heads would severely decrease the performance of the model (more than 30% accuracy drop). 
And our design of the compensation token could essentially fix this problem and condense all the dropped information in those heads.
In summary, using the retrieval heads defined in [1] solely is not enough for lossless KV cache compression (although it is indeed an important component of our algorithm design). We will add a discussion section about this in our revised version.

Q4: Elaborate on the “Retrieve and Process” hypothesis.
The “Retrieve and Process” hypothesis originates from the observed behavior of attention heads in LLMs. Most attention heads focus primarily on recent tokens, while only a few retrieval heads attend to remote tokens to extract relevant information. This mirrors the human cognitive process: recalling pertinent information before generating a response. We will clarify this point in our revised version.

Q5: What does "Protect" mean:
In line 234, “protect” refers to preserving the KV cache in selected heads while discarding remote tokens in others. 

# To R3
Q1: More experimental results:
xxx

Q2: The exact compression ratio:
xxx

Q3: Novelty of RazorAttention:
Actually we are the first work that applies the induction heads for KV cache compression and we proposed two essential strategies beyond the inductive heads in order to achieve a lossless performance. The idea of inductive heads was originally proposed in [1] where authors noticed that several certain attention heads follow a retrieving pattern (attend to the previous existing tokens). The concurrent work [2] noticed that this phenomenon also exists under a lengthy input, and their ablation studies proved that these inductive heads are more important than the other heads under long context. There are two major contributions of our work:
1) The definition of the retrieval head is different: In [1], authors only considered the induction heads as the retrieval heads. Our analysis reveals that the copy heads are also very essential
for retaining all the performance (see Figure 5 in our paper). Using the retrieval heads defined in [1] results in about 10% of accuracy drop when tested using Needle In A Haystack.
2) The design of the compensation token: Directly discarding all the remote tokens in the non-retrieval heads would severely decrease the performance of the model (more than 30% accuracy drop). 
And our design of the compensation token could essentially fix this problem and condense all the dropped information in those heads.
We will add a discussion section about this part in our revised version.

# To R4
Q1: The efficiency evaluation:
xxx

Q2: Overhead Analysis
xxx

Q3: More comparison
xxx
Noticed that our algorithm can be perfectly combined with current token-dropping algorithms, such as H2O, SnapKV, MInference. Since we only constrain the cache budget in the short heads while in long heads the KV cache can be further compressed using these algorithms.

Q4: Result of RA under different compression ratios:
Actually the reason why we only consider one specific ratio is that we found the ratio of the necessary retrieval heads is about 15%. Using fewer retrieval heads would introduce certain accuracy drop (see Table-4 in our paper) while using more heads gives no more accuracy gain. Therefore we only report the comparison result under this compression ratio for all baselines. We will clarify this in our revised version.


# To R5
Q1: Novelty of RazorAttention:
Actually we are the first work that applies the induction heads for KV cache compression and we proposed two essential strategies beyond the inductive heads in order to achieve a lossless performance. The idea of inductive heads was originally proposed in [1] where authors noticed that several certain attention heads follow a retrieving pattern (attend to the previous existing tokens). The concurrent work [2] noticed that this phenomenon also exists under a lengthy input, and their ablation studies proved that these inductive heads are more important than the other heads under long context. There are two major contributions of our work:
1) The definition of the retrieval head is different: In [1], authors only considered the induction heads as the retrieval heads. Our analysis reveals that the copy heads are also very essential
for retaining all the performance (see Figure 5 in our paper). Using the retrieval heads defined in [1] results in about 10% of accuracy drop when tested using Needle In A Haystack.
2) The design of the compensation token: Directly discarding all the remote tokens in the non-retrieval heads would severely decrease the performance of the model (more than 30% accuracy drop). 
And our design of the compensation token could essentially fix this problem and condense all the dropped information in those heads.
We will add a discussion section about this part in our revised version.

There are some related works identifying the different patterns of retrieval and non-retrieval heads, such as DuoAttention, Retrieval Head Mechanistically Explains Long-Context Factuality, MInference, etc. It would be better to clarify the novelty of the proposed method.
It can only be applied to multiple KV cache scenarios. For only single KV cache model structure, such as YOCO, it cannot be applied.
The baselines are not comprehensive. It would be beneficial if the authors could compare their results with more advanced baselines.
It seems the proposed method cannot achieve the best performance in all the benchmarks. It would be beneficial to give detailed analysis of the worse cases.



