# LOCALITY SENSITIVE HASHING - LSH

---

The task of finding nearest neighbours is very common. You can think of applications like finding duplicate or similar documents, audio/video search. Although using brute force to check for all possible combinations will give you the exact nearest neighbour but it’s not scalable at all. Approximate algorithms to accomplish this task has been an area of active research. Although these algorithms don’t guarantee to give you the exact answer. These algorithms are faster and scalable.

Locality sensitive hashing (LSH) is one such algorithm. LSH has many applications, including:

- **Near-duplicate detection:** LSH is commonly used to deduplicate large quantities of documents, webpages, and other files.
- **Genome-wide association study:** Biologists often use LSH to identify similar gene expressions in genome databases.
- **Large-scale image search:** Google used LSH along with PageRank to build their image search technology VisualRank.
- **Audio/video fingerprinting:** In multimedia technologies, LSH is widely used as a fingerprinting technique A/V data.

Let's an example the below picture 

![lsh1](https://miro.medium.com/v2/resize:fit:640/format:webp/1*9-XHwDPzGqky2O3JdOU9IA.png)

LSH refers to a family of functions (known as LSH families) to hash data points into buckets so that data points near each other are located in the same buckets with high probability, while data points far from each other are likely to be in different buckets. This makes it easier to identify observations with various degrees of similarity.

---

### Story of LSH

When we consider the complexity of finding similar pairs of vectors, we find that the number of comparisons required to compare everything is unmanageably enormous even with reasonably small datasets.

Let’s consider a vector index/list of element. If we were to introduce just one new vector and attempt to find the closest match — we must compare a vector to every other vector in our database. This gives us a linear time complexity — which cannot scale to fast search in larger datasets.

The problem is even worse if we wanted to compare all of those vectors against each other, the comparisons are getting to big and complex to handle.

So we need a way to reduce the number of comparisons. Ideally, we want only to compare vectors that we believe to be potential matches — or candidate pairs.

**Here LSH comes into the picture.....**

At its core, the final LSH function allows us to segment and hash the same sample several times. And when we find that a pair of vectors has been hashed to the same value at least once , we tag them as candidate pairs — that is, potential matches.

A typical hash function aims to place different values (no matter how similar) into separate buckets. As shown below

![trad hash](https://cdn.sanity.io/images/vr8gru94/production/faaae9e55d33b08629574243d8456446650df096-1400x787.png)

However, there is a key difference between this type of hash function and that used in LSH. In traditional hashing we want to minimize the collision, but LSH is almost the opposite. In LSH, we want to maximize collisions — although ideally only for similar inputs. As mentioned below 

![lsh2](https://cdn.sanity.io/images/vr8gru94/production/862f88182a796eb16942c47d93ee03ba4cdaee4d-1920x1080.png)

---

Mainly the LSH method can be implemented by 3 methods as shingling, minhashing and banding.

![lash3](https://cdn.sanity.io/images/vr8gru94/production/89413953597fbfdd36c4fa77ca0eeafaf6cf944a-1280x980.png)

1) K-Shingling:  Shingling is the process of breaking text or data into overlapping sequences (shingles) of a fixed length. In text processing, shingles are often words or tokens.
Example:
Input text: "To be or not to be"; 
Shingles of length 2: ["To be", "be or", "or not", "not to", "to be"]

2) 