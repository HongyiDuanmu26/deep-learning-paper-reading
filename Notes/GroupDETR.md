# [Group DETR: Fast DETR Training with Group-Wise One-to-Many Assignment](https://arxiv.org/pdf/2207.13085)

The author paid attention to Hungarian matching in DETR-like models. They proposed a group method to improve its stability.

<img width="1388" height="478" alt="image" src="https://github.com/user-attachments/assets/ff0c21ae-de21-428f-979a-c73da48d3be2" />

(though the authors said they are (a) in the Fig. 2, but I think (b) is closer to what they want. Duplicated queries will be passed into one decoder, 
or parallelly passed into decoders and share weights)

If the model is set with N queries. Make it K groups, and you will have KxN queries in total. Passed them in parallel into one decoder with shared weights. 
N output predicted objects in each group will be Hungarian matched with N' GTs individually.

This one-to-many will make training more supervised as GTs are used more times in one iteration. 
