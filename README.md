# KDAL
- Knowledge Distillation-based Active Learning for Anomaly Detection
- [The 2024 Spring Joint Conference of the Korean Institute of Industrial Engineers](https://kiie.org/Conference/ConferenceView.asp?AC=0&CODE=CC20240101&CpPage=248#CONF)
- Associated with [Industrial Artificial Intelligence Lab](https://iai.seoultech.ac.kr/index.do)

#### 📍 Project Objective
This study utilizes a mixture of normal and abnormal data (Unlabeled Dataset) to reflect the real data distribution in the Active Learning methodology.

#### 📍 Project Duration
2023.11 ~ 2024.05

#### 📍 Team Member

<table style="border: 0.5px solid gray">
 <tr>
    <td align="center"><a href="https://github.com/jeewonkimm2"><img src="https://avatars.githubusercontent.com/u/108987773?v=4" width="130px;" alt=""></td>
    <td align="center" style="border-right : 0.5px solid gray"><a href="https://github.com/bae-sohee"><img src="https://avatars.githubusercontent.com/u/123538321?v=4" width="130px;" alt=""></td>

  </tr>
  <tr>
    <td align="center"><a href="https://github.com/jeewonkimm2"><b>Jeewon Kim</b></td>
    <td align="center" style="border-right : 0.5px solid gray"><a href="https://github.com/bae-sohee"><b>Sohee Bae</b></td>
  </tr>
</table>
<br/>


## 1. Overview
- Anomaly Scores derived from Unsupervised Anomaly Detection(UAD) are used to compose the initial data pool of Active Learning with samples rich in information.
- Through this, the model quickly grasps various patterns of the data in the initial learning stage and achieves high performance in the early learning stage, thus saving labeling costs.
  <img width="1085" alt="Screenshot 2024-05-28 at 12 13 11 PM" src="https://github.com/jeewonkimm2/KDAL/assets/108987773/823aee57-01c5-4870-95c9-64a48bd68e1c">

  #### Overall Structure
  1. Derive anomaly scores from the unlabeled pool using UAD.
  2. Replace the probability values of soft labels from knowledge transfer [6] with the anomaly scores to build an initial classification model.
  3. Select the K data points with the lowest confidence in Cycle 0 using the Least Confidence method of uncertainty sampling as initial labeling targets.
  4. Retrain the model with the initial labeled data and label the K data points.
  5. Train the classification model using the sum of the soft loss of the entire dataset, derived from anomaly scores, and the hard loss of the labeled pool.
  6. Label K data points using Least Confidence, then add them to the labeled pool.
  7. Repeat steps [v-vi] until the termination condition of active learning is met.
 
  #### Anomaly Score
  - In this experiment, OCSVM was adopted, but other UADs can be used to derive anomaly scores. Normalize and sign-transform the anomaly scores to have values between 0 and 1, where a value closer to 1 indicates a higher likelihood of being an anomaly.
 
  #### Training Loss
  - Soft Loss: The difference between the soft target probabilities obtained from the anomaly scores and the output of the classification model.
  - Hard Loss: The cross-entropy loss between the output of the classification model and the actual labels in the labeled dataset.
  - Total Loss: In Cycle 0, only the soft loss is used. In Cycle n, both the soft loss and the hard loss are used.
 
 ## 2. Environment
- Python version is 3.9.
- Installing all the requirements may take some time. After installation, you can run the codes.
- Please notice that we used 'PyTorch' and device type as 'GPU'.
- We utilized 2 GPUs in our implementation. If the number of GPUs differs, please adjust the code accordingly based on the specific situation.
- ```requirements.txt``` file is required to set up the virtual environment for running the program. This file contains a list of all the libraries needed to run your program and their versions.

    #### In **Anaconda** Environment,

  ```
  $ conda create -n [your virtual environment name] python=3.9
  
  $ conda activate [your virtual environment name]
  
  $ pip install -r requirements.txt
  ```

  - Create your own virtual environment.
  - Activate your Anaconda virtual environment where you want to install the package. If your virtual environment is named 'testasal', you can type **conda activate testasal**.
  - Use the command **pip install -r requirements.txt** to install libraries.
