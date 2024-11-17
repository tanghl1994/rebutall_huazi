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

