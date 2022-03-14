# SVM-for-object-detection
To detect human hands in images using SVM

**Goal**: The motto of this project is to detect human hands in images, for this we need a classifier that can distinguish between hand image patches from non-hand patches. To train such a classifier, we can use SVMs. To do this deep features are extracted from detectron2 https://github.com/facebookresearch/detectron2. 

The training data is typically a set of images with bounding boxes of the hands. Positive training examples are image patches extracted at the annotated locations. A negative training example can be any image patch that does not significantly overlap with the annotated hands. Thus there potentially many more negative training examples than positive training examples. Due to memory limitation, it will not be possible to use all negative training examples at the same time. To handle this, I have implemented hard-negative mining to find hardest negative examples and iteratively train an SVM.

**Data:** Downloaded the ContactHands dataset from http://vision.cs.stonybrook.edu/ ̃supreeth/ContactHands_data_website/
**Reference:** ‘Detecting Hands and Recognizing Physical Contact in the Wild.’ S. Narasimhaswamy, T. Nguyen, M. Hoai. Advances in Neural Information Processing Systems (NeurIPS), 2020.

**Hard negative mining algorithm **

PosD ← all annotated hands
NegD ← random image patches
(w, b) ← trainSVM(P osD, N egD)
for iter = 1,2,··· do
    A ← All non support vectors in NegD.
    B ← Hardest negative examples
    NegD ← (NegD \ A) ∪ B.
    (w, b) ← trainSVM(P osD, N egD)
 end for
