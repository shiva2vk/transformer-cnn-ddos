
# Hybrid Transformer-CNN Architecture for DDoS Attack Detection


## Abstract: 
Distributed Denial-of-Service (DDoS) attacks pose a significant threat to modern networks, often overwhelming critical infrastructure with malicious traffic. Traditional detection techniques, while effective against known patterns, struggle to generalize across evolving attack vectors. This study introduces a novel hybrid deep learning model combining Transformer and Convolutional Neural Network (CNN) architectures—leveraging the Transformer’s ability to capture long-range dependencies and CNN’s efficiency in local pattern extraction. Trained and evaluated on the CIC-DDoS2019 dataset, my TransformerCNN model achieves near-perfect detection accuracy with minimal false positives. Performance metrics such as precision, recall, F1-score, and AUC confirm the model's robustness and generalization. The results highlight the potential of attention-based hybrid architectures for next-generation intrusion detection systems (IDS), especially in the context of high-volume cyberattacks like DDoS.


Distributed Denial of Service (DDoS) is a type of cyberattack in which multiple compromised systems, often part of a botnet, are used to flood a target (such as a server, website, or network) with overwhelming traffic. The goal is to disrupt normal services, making the system unavailable to legitimate users. for example :

    -A botnet—a network of malware-infected devices—is controlled by the attacker.
    -These bots simultaneously send massive volumes of data or requests to the target.
    -The target server or application becomes overloaded, crashes, or becomes unresponsive.

## Types of DDoS Attacks
    -Volume-Based Attacks: Overwhelm the target with high-bandwidth traffic (e.g., UDP floods).
    -Protocol Attacks: Exploit vulnerabilities in network protocols (e.g., SYN floods).
    -Application-Layer Attacks: Target web applications by mimicking legitimate user behavior (e.g., HTTP GET/POST floods).


As Distributed Denial of Service (DDoS) attacks become more sophisticated, traditional rule-based systems struggle to keep up. To address this, deep learning models—particularly hybrid architectures like TransformerCNN—are emerging as powerful tools in cyber threat detection, including early identification of DDoS attacks.

TransformerCNN : TransformerCNN is a hybrid neural network architecture that combines the strengths of Convolutional Neural Networks (CNNs) for extracting spatial and local feature patterns in network traffic, and Transformers for capturing long-range dependencies and global context using self-attention mechanisms. This combination is ideal for learning complex patterns in network flow data.
DDoS attacks are no longer simple volumetric floods. Modern attacks are multi-vector, distributed, and often mimic legitimate traffic patterns. Traditional detection techniques, including signature-based systems or simple statistical thresholds, often fail to generalize to unseen or adaptive attacks. TransformerCNN offers a hybrid deep learning architecture that effectively addresses the challenges posed by complex, high-volume, and temporally-dependent DDoS attack patterns. CNN captures local spike patterns, like packet rate bursts or abrupt protocol flag changes. Transformer captures temporal dependencies, such as gradual buildup in traffic or coordinated botnet behavior over time.

This dual mechanism allows TransformerCNN to learn both short-term anomalies and long-term intent in traffic flows. Transformers excel at modeling such sequences via self-attention, identifying long-range relationships better than LSTMs or GRUs. This is crucial for detecting stealthy or low-and-slow DDoS attacks. Unlike RNNs, the Transformer block allows parallel computation, which leads to faster training and inference, critical for real-time DDoS defense at scale

TransformerCNN can easily be extended to multi-class classifiers, learning subtle feature combinations that distinguish different attack types. TransformerCNN is not just a neural network choice — it is a strategic architecture for modern DDoS defense. Its ability to understand both what is happening (via CNN) and how it evolves over time (via Transformer) gives it a unique edge in handling the complex, evolving nature of attacks represented in the CICDDoS2019 dataset.

## TransformerCNN Architecture

1. Input Layer
    Input shape: (batch_size, sequence_length, feature_dim)

    Example: for a single network flow with 70 features, sequence_length = 1, feature_dim = 70.

    Input can be derived from network flow features, such as packet length, duration, number of bytes, header flags, protocol types, etc.

2. Linear Embedding Layer
    A Linear(input_dim, d_model) layer is used to project the raw input features to a higher-dimensional space.

    Example: Linear(70, 128)

    This embedding acts as a form of learned feature projection.

3. Positional Encoding (Optional)
    If the input sequence length > 1 (e.g., a sequence of packets), positional encodings help the model preserve the order of traffic patterns.

    Either sinusoidal or learnable positional embeddings can be used.

4. Transformer Encoder Layers
    A stack of multi-head self-attention layers followed by feed-forward layers.
    These layers model long-range dependencies and detect patterns across multiple traffic flows.

    Each Transformer block includes: Multi-head Attention: Self-Attention(Q, K, V), Add & Norm, Feed-Forward Network (FFN), Add & Norm

    what is role for DDoS:
    Transformers can detect coordinated or distributed attack behavior over time and across different IP flows (e.g., in a botnet-driven flood).

5. CNN Stack (Convolutional Layers)
Input is reshaped from [batch_size, seq_len, d_model] to [batch_size, d_model, seq_len] and fed to Conv1D.

    Layers:

    Conv1D: captures local patterns like sudden packet bursts.

    ReLU activation

    MaxPooling or AdaptiveAvgPooling for spatial reduction

    what is role for DDoS:
    CNNs excel at identifying spikes, bursts, or anomalous packet structures at the feature level, such as sudden SYN floods or irregular byte counts.

6. Fully Connected (Dense) Layers
    After CNN layers, the flattened output goes into a multi-layer perceptron (MLP).

    Common stack:

    Linear(32, 64)

    ReLU + Dropout

    Linear(64, num_classes)

    num_classes = 2 for Benign vs DDoS, or more for multi-class attack detection.

7. Output
    A softmax or sigmoid layer depending on whether the task is binary or multi-class classification.

    Produces a probability distribution over class labels.



## Dataset used for this project is [CICDDoS2019](https://www.unb.ca/cic/datasets/ddos-2019.html) 
Dataset: The CICDDoS2019 dataset, developed by the Canadian Institute for Cybersecurity (CIC), is a benchmark dataset designed to support the development and evaluation of DDoS detection systems using machine learning and deep learning techniques.It was created to address the limitations of earlier datasets by providing realistic, diverse, and labeled traffic data collected in a controlled yet representative environment

    CICDDoS2019 dataset contains 7 major categories of DDoS attacks, including:

    UDP Flood
    ICMP Flood
    SYN Flood
    HTTP Flood
    Slowloris Attack
    DNS Flood
    NTP Flood




## Performance results
- **Accuracy**: 99.9%
- **AUC-ROC**: 0.9999
- **Precision/Recall**: Near-perfect
- **Confusion Matrix**:
  - True Positives: 11260
  - False Positives: 9
  - True Negatives: 11218
  - False Negatives: 13