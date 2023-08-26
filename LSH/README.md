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

- **K-Shingling:**  Shingling is the process of breaking text or data into overlapping sequences (shingles) of a fixed length. In text processing, shingles are often words or tokens.
Example:
Input text: "To be or not to be"; 
Shingles of length 2: ["To be", "be or", "or not", "not to", "to be"]


- **Jaccard Index:** We’ve a representation of each document in the form of shingles. Now, we need a metric to measure similarity between documents. Jaccard Index is a good choice for this. Jaccard Index between document A & B can be defined as:
> Jaccard Index = (A INTERSECTION B) / (A UNION B)

As an example - 
Suppose A: “Nadal” and B: “Nadia”, then 2-shingles representation will be:
A: {Na, ad, da, al} and B: {Na, ad, di, ia}

Then Jaccard Index = (common singles in A&B) / (Total Singles in A&B) = 2/6

- **Min Hashing:** Minhashing is a technique to estimate the Jaccard similarity between sets by using hash functions. It involves selecting a set of hash functions and finding the minimum hash value for each shingle in a document. This is the critical and the most magical aspect of LSH algorithm.

Step 1: Random permutation (π) of row index of document shingle matrix.

![mh1](https://miro.medium.com/v2/resize:fit:640/format:webp/1*_o4WE7tW0qIjefYzjlpqJA.png)

Step 2: In LSH hash function is the index of the first (in the permuted order) row in which column C has value 1. Do this several time (use different permutations) to create signature of a column. Follow the below Image.

![mf2](https://miro.medium.com/v2/resize:fit:828/format:webp/1*wkjgq-9UPR2rDjQ68bI9tg.png)

Also if we see some more example then:

![mh4](https://miro.medium.com/v2/resize:fit:1374/format:webp/1*JSPUfzDnwwxt7tjsxOCchw.png)


- **Min-hash property**:
The similarity of the signatures is the fraction of the min-hash functions (rows) in which they agree. So the similarity of signature for C1 and C3 is 2/3 as in C1 and C3 the R1 and R3 elements are same. So, 1st and 3rd row are same.

![mh5](https://miro.medium.com/v2/resize:fit:440/format:webp/1*WKNfxHCviPHTFavK_dQflw.png)
---

## LSH Main Concept:

The general idea of LSH is to find an algorithm such that if we input signatures of 2 documents, it tells us that those 2 documents form a candidate pair or not i.e. their similarity is greater than a threshold t.

If 2 documents hash into same bucket for at least one of the hash function we can take the 2 documents as a candidate pair

### Band Partition:
We will be taking the banding approach to LSH — which we could describe as the traditional method. It will be taking our signatures, hashing segments of each signature, and looking for hash collisions.

The banding method solves this problem by splitting our vectors(i.e. signeture Matrix) into sub-parts called bands b. Then, rather than processing the full vector through our hash function, we pass each band of our vector through a hash function.

Imagine we split a 100-dimensionality vector into 20 bands. That gives us 20 opportunities to identify matching sub-vectors between our vectors.

![band](https://cdn.sanity.io/images/vr8gru94/production/1b1fc2e08469ea024573078b275f7228e2e7d824-1920x1080.png)

We split our signature matrix into b sub-vectors, each is processed through a hash function (we can use a single hash function, or b hash functions) and mapped to a hash bucket.

**Here is the algorithm for banding:**

- Divide the signature matrix into b bands, each band having r rows.
- For each band, hash its portion of each column to a hash table with k buckets.
- Candidate column pairs are those that hash to the same bucket for at least 1 band.
- change the b and r to catch most similar pairs but few non-similar pairs.

Follow the below representation for more clarity:

![band2](https://miro.medium.com/v2/resize:fit:828/format:webp/1*NdAEYKfMLikerNpXjXv8FQ.png)

> **Similarity Threshold:**
> If we take b large i.e more number of hash functions, then we reduce r as b*r is a constant (number of rows in signature matrix is same always). Intuitively it means that we’re increasing the probability of finding a candidate pair. This case is equivalent to taking a small t.

Example of value for b & r:

Let’s say your signature matrix has 100 rows. Consider 2 cases:

b1 = 10 → r = 10

b2 = 20 → r = 5

In 2nd case, there is higher chance for 2 documents to appear in same bucket at least once as they have more opportunities (i.e. more no of bands) (20 vs 10) and fewer elements of the signature are getting compared (5 vs 10).

Higher b implies lower similarity threshold (higher false positives) and lower b implies higher similarity threshold (higher false negatives)

---

## Subsampling using LSH:

LSH can be used in subsampling to efficiently select a subset of data points from a larger dataset while maintaining the desired characteristics of the original data distribution. Subsampling is especially useful when dealing with large datasets, as it reduces the amount of data that needs to be processed while still preserving the essential features of the data.

Here's how LSH can be used in subsampling:

1. **Data Representation:**
   Represent your data points in a suitable format. This could be vectors, features, documents, etc., depending on your application.


2. **MinHash and Signature Generation:**
   Apply MinHashing to generate signatures for each data point. These signatures are used to represent the data points while maintaining locality-sensitive properties. Each data point's signature is a compact representation of its features.


3. **Banding and Hashing:**
   Divide the signatures into bands, and apply hash functions to hash the signatures to buckets within each band. This process groups similar data points together with high probability.


4. **Subsampling Process:**
   Choose a few buckets from each band to form a set of candidate buckets. These candidate buckets are potential representatives of the data distribution. The goal is to select data points from these buckets as your subsample.


5. **Data Point Selection:**
   For each selected candidate bucket (candidate buckets are those that you will consider for subsampling.), choose the bucket that has more no of item frequencies, or else it can be chosen upon the requirements or randomly.


6. **Benefits of LSH in Subsampling:**
   - **Efficiency:** LSH allows you to quickly identify candidate buckets that likely contain similar data points. This reduces the search space for selecting your subsample.
   - **Preservation of Characteristics:** LSH aims to group similar data points together. By selecting data points from the candidate buckets, you are likely to capture the diversity and essential characteristics of the original data distribution.
   - **Scalability:** LSH makes subsampling feasible even for large datasets by reducing the number of data points you need to examine while still maintaining a representative subsample.

**Example:**

Imagine you have a large dataset of images and you want to create a smaller representative subset for training a machine learning model. You can apply LSH to the image feature vectors to group similar images together. Then, you select a few images from each candidate bucket to form your subsample. This way, you capture the diversity of images while significantly reducing the number of images you need to include in your training set.

