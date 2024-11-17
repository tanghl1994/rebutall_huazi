# To R1
We sincerely appreciate your constructive feedback of our paper, and we will make certain updates w.r.t. your concerns in our revised version, especially more discussion of Induction Heads as it's a fundamental basis of our algorithm design. 

Q1: Comparison with SnapKV:
Yes, recent work, such as SnapKV are quite compatitive algorithms for KV cache compression. We indeed find that SnapKV achieves quite a promising compression ratio and accuracy when the use
query is known before generation. Below we present the results of SnapKV with and without the user queries given:
xxx
xxx
As we can see, the results of SnapKV is greatly devastcated when it has no idea about the user query, unlike RazorAttention. We believe this discussion is a quite important part and will calrify this in our revised version.

Q2: Detailed setting of experiments and more long-context benchmarks:
We do notice about 2% of accuracy fluctutaion of Needle In A Haystack under different prompt (although we use the same prompt for all baselines and RazorAttention). We will clarify our input prompt in our revised version.
Meanwhile, Here we inlcude the full results of RazorAttention on Longbench
xxx
As you suggest, we have included the results of xxx below:
xxx.

Q3: The efficiency evaluation:
xxx

# To R2
Q1: More experiments details:
For H2O, we set the KV cache budget the same as the RazorAttention for each sentence for a fair comparison. We appreciate your suggestion and will clarify this in our revised version.

Q2: The efficiency evaluation:
xxx
Lack of overhead evaluation on efficiency The paper claims that RazorAttention can enhance LLM inference efficiency without overhead. However, there is no efficiency evaluation (both peak memory usage as well as the inference latency analysis). It is essential to have such real efficiency gain evaluation.

Q3: Comparison with previous work [1]:
Yes, the concurrent work [1] also found that several "retrieval heads" contribute most to the long inputs, and there are two major contributions of our work:
1) The definition of the retrieval head is different: In [1], authors only considered the induction heads as the retrieval heads. Our analysis reveals that the copy heads are also very essential
for retaining all the performance (see Figure 5 in our paper). Using the retrieval heads defined in [1] results in about 10% of accuracy drop when tested using Needle In A Haystack.
2) The design of the compensation token: Directly discarding all the remote tokens in the non-retrieval heads would severely decrease the performance of the model (more than 30% accuracy drop). 
And our design of the compensation token could essentially fix this problem and condense all the dropped information in those heads.
In summary, using the retrieval heads defined in [1] solely is not enough for lossless KV cache compression (although it is indeed an important component of our algorithm design). We will add a discussion section about this in our revised version.

Q4: Clarification of the "Retrieve and Process" hypothesis:
Here the "Retrieve and Process" hypothesis is derived from the fact that many attention heads put most of their attention weights to the recent tokens, and only several retrieval heads can essentially retrieve the remote information by putting a large attention weight to those tokens. This process simulates the human processing of a long context where we  first recall all the related information and then generate the final response. We will clarify this point in our revised version.

Q5: What does "Protect" mean:
In line 234, the protect means that we keep the KV cache in those heads as the original while we discard the remote tokens in the rest heads.

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



