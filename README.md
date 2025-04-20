# Rootkit-Detection-LSTM
McAfee's Rootkit Detective is also used to detect system hooks and log outliers, which produces a rich dataset of 973 system behavioral features for training and testing. This approach applies preprocessing methods like data normalization and feature selection based on chi-square to improve model performance

<img width="556" alt="Screenshot 2025-04-20 at 2 52 28 PM" src="https://github.com/user-attachments/assets/61dfd548-0ce7-45b0-811e-46a5993ad0cb" />

LSTM networks, a variant of Recurrent Neural Networks (RNNs), are specifically designed to work well with sequential data. Since rootkits typically cause incremental and fine-grained changes to system states instead of making abrupt modifications, maintaining long-term dependencies is essential. LSTMs address the shortcomings of standard RNNs by avoiding the vanishing gradient issue and selectively forgetting or remembering information via a gated mechanism. In our work, the LSTM encoder is employed for processing time-series data obtained from system activity. It accepts sequences of features as input, where each feature is a representation of an aspect of system behavior, like import address table (IAT) manipulations, SSDT hooks, and function return values (RV). The LSTM encoder learns temporal patterns in these behaviors and represents them as a latent representation (Z) that embodies the system's normal state of operation.
The forget gate is helpful in determining which information from the previous state should be discarded. This is crucial in rootkit detection, since it helps remove irrelevant patterns while focusing on significant deviations. This can be visualized as:

ft=σ(Wf . [ht-1,xt] + bf)

where
ft : Forget gate activation vector.
 : Sigmoid activation function.
Wf :Weight matrix for the forget gate.
ht-1 : Hidden state from the previous timestep.
xt : Input at the current timestep.
bf : Bias term for the forget gate.

The Input Gate increments the cell state with new information, ensuring that changes in system activity are learned by the model.
input gate
it=σ(Wi . [ht-1,xt] + bi)
Ct~=tanh(Wc .[ht-1,xt] +bc) 
Ct= ft Ct-1 +  it ⊙ Ct~ 

​
where:
it : Input gate activation.
Ct~ : Candidate cell state.
Ct : Updated cell state.
The Output Gate controls the exposure of the internal memory to the next layer in the network, allowing key information to be passed forward.
				ot  = σ(Wo  ​⋅ [ht-1, xt] + bo )
ht =  ot tanh(Ct)

where:
ot : Output Gate Activation
Wo : Weight Matrix for Output Gate
ht-1 : Previous Hidden State
xt : Current Input
 bo  : Bias for Output Gate
Ct​ : Current Cell State
ht​ : Current Hidden State
σ : Sigmoid Function
tanh : Hyperbolic Tangent Function

<img width="561" alt="Screenshot 2025-04-20 at 2 55 53 PM" src="https://github.com/user-attachments/assets/96bf22fc-c46e-478f-8770-15ea9a154f52" />
<img width="476" alt="Screenshot 2025-04-20 at 2 56 30 PM" src="https://github.com/user-attachments/assets/a3a1d90b-426d-4964-92f1-b50518ea49c9" />

