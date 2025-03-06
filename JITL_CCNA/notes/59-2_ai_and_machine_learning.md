# AI & Machine Learning

## Machine Learning

- **Machine Learning** (ML) is a subset of AI that focuses on enabling computers to learn from and improve with the need for explicit programming
- Examples include (but not limited to):
  - Email spam filtering
  - Personalised product recommendations
  - Fraud detection (banking)
  - Natural language processing (NLP)
- ML is the driving force behind many modern AI applications

### Types of ML

- **Supervised Learning**
  - The model is trained on labeled data, where the correct answers are provided, to make predictions or classifications on new data
- **Unsupervised Learning**
  - The model is given unlabeled data and tasked with finding patterns, relationships, or groupings within the data
- **Reinforcement Learning**
  - The model learns by interacting with an environment, receiving rewards or penalties based on its actions to maximize its performance over time
- **Deep Learning**
  - A specialised subset of ML that uses multi-layered neural networks to handle datasets and perform complex tasks like image recognition and natural language processing

#### Supervised Learning

- Supervised learning trains the model on a labeled dataset
  - Each input provided to the model for training has a corresponding label
  - By examining these labeled examples, the model learns the relationship between the data and the given label
- By training on labeled examples, the model learns to classify new, unseen data with high accuracy
- **Advantages**
  - Highly accurate when labeled data is available
  - Straightforward to understand and implement
- **Disadvantages**
  - Requires large, labeled datasets, which can be expensive and time-consuming to create
  - Output is limited to the labels in the training data

#### Unsupervised Learning

- Unsupervised learning trains the model on an unlabeled dataset
  - No predefined labels are provided
  - The model can discover patterns, structures, or relationships within the data (without human supervision)
    - It groups (clusters) similar data points based on shared features
- **Advantages**
  - No need for labeled data
  - Reveals hidden patterns
- **Disadvantages**
  - Interpretation and labelling of the results is required
  - Less accurate

#### Reinforcement Learning

- Reinforcement learning trains a model by rewarding or penalizing its actions in a given environment to maximise performance over time
  - The model learns to take actions that achieve the highest reward or best outcome
- How it works:
  - The model (called an agent) interacts with an environment
  - It takes an action and receives feedback (reward or penalty)
  - Over time, it learns which actions lead to the best results
- **Advantages**
  - Capable of learning complex behaviours
  - Adapts to dynamic environments
- **Disadvantages**
  - Resource intensive
  - Risk of suboptimal learning if the reward system isn't properly designed

#### Deep Learning

- Deep learning uses artificial neural networks to process and learn from large and complex datasets
  - An *artificial neural network* is a computational model inspired by how biological neural networks like the human brain process information
  - Data is passed through multiple layers of nodes (neurons), with each layer extracting increasingly abstract features
  - The neural network can be trained using, supervised, unsupervised, and/or reinforcement methods
- **Advantages**
  - Deep learning excels at handling large, unstructured datasets like images, audio, and text
  - Achieve state-of-the-art performance in tasks like image recognition, natural language processing (NLP), and autonomous driving
- **Disadvantages**
  - Resource intensive
  - The model can be a "black box", making it difficult to interpret how it arrives at its decisions

## Predictive and Generative AI

- **Predictive AI**
  - Uses machine learning to analyse historical data and predict future outcomes or trends
    - Examples: security anomaly detection, weather forecasting, etc.
- **Generative AI**
  - Uses machine learning to learn patterns from existing data and create new content, such as text, images, or audio

### Predictive AI

- Analyses historical data to forecast future outcomes, trends, or events
  - The model identifies patterns and correlations past data
  - The learned patterns are applied to make predictions
- **Applications** include:
  - Healthcare: Predicting patient outcomes or disease progression
  - Network security: Detecting anomalies that might indicate a potential threat or failure
  - Traffic management: Predicting congestion based on historical and real-time traffic data
    - Applies to both vehicles and network traffic
  - Business forecasting: Predicting sales trends or customer behaviour
  - Weather behaviour: Analysing meteorological  data to predict weather conditions
- **Advantages**
  - Improves decision-making by providing actionable insights
  - Detects potential problems before they occur
- **Disadvantages**
  - Requires high-quality, relevant historical data
  - Accuracy depends on how well the patterns in past data generalise to new scenarios

### Generative AI

- Generative AI learns patterns from existing data and creates new content such as text, images, and audio
  - It focuses on producing outputs that resemble the input that it was trained on
- **Applications** include:
  - Text generation: ChatGPT, Gemini, Copilot
  - Image generation: Midjourney, DALL-E
  - Video generation: Sora (OpenAI), Veo 2 (Google)
- **Advantages**
  - Great for creative tasks where human input is limited or time-consuming
  - Enables automation of content creation across various fields
- **Disadvantages**
  - Risk of misuse
  - Generated content is only as good as the quality of the training material
  - Hallucinations, the model can appear to "make up content"

### Predictive and Generative AI in Networks

- **Predictive AI**
  - **Traffic forecasting**
    - Predict network traffic patterns to optimise bandwidth allocation and prevent congestion
  - **Security threat detection**
    - Identify anomalies or suspicious patterns in real-time to mitigate potential security threats
  - **Predictive maintenance**
    - Anticipate hardware failures by analysing historical and current performance data, reducing downtime

- **Generative AI**
  - **Network documentation**
    - Generate documentation about network configurations, policies, etc.
  - **Configuration generation**
    - Automatically generate configurations for network devices based on desired policies and requirements
  - **Network design**
    - Suggest optimised network layouts or modifications tailored to specific business needs and workloads
  - **Troubleshooting**
    - Produce solutions or diagnostics based on log files or error messages to resolve issues efficiently
  - **Script generation**
    - Automatically generate network automation scripts

## AI in Cisco Catalyst Center

- Cisco **Catalyst Center** (formerly DNA Center) features a variety of AI-enabled features to:
  - Identify issues before they impact users
  - Reduce the time required to resolve issues
  - Increase the performance and security of the network
- Features include
  - **AI Network Analytics**
    - Uses AI to establish the baseline behaviour of the network
    - Provides insights and recommendations for optimising network performance
    - Continuously monitors the network to predict and detect anomalies
  - **Machine Reasoning Engine (MRE)**
    - Uses AI to perform root-cause analysis when network issues arise
    - Suggests resolutions or takes automated corrective actions without requiring manual intervention
    - Reduces downtime by identifying and resolving issues faster than traditional methods
  - **AI Endpoint Analytics**
    - Identifies and classifies devices on the network, providing detailed visibility
    - Detects unauthorised devices or unusual behaviour
    - Simplifies device onboarding by automating profiling and segmentation
  - **AI-enhanced Radio Resource Management (RRM)**
    - Optimises wireless network performance by dynamically adjusting radio settings
    - Uses AI to balance load, reduce interference, and improve coverage across wireless access points
