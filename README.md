# Spam Email Classifier 🚀
# 🔍 Overview
Spam Email Classifier is a machine learning-based application that detects and classifies emails as spam or not spam. Using Natural Language Processing (NLP) techniques, the model analyzes the content of an email and predicts whether it is a legitimate message or spam.

## 🛠️ Technologies Used
- Python 🐍
- Scikit-Learn (Machine Learning)
- NLTK / SpaCy (Natural Language Processing)
- Pandas & NumPy (Data Processing)
- Pickle (Model Serialization)
- Flask / Streamlit / AWS (For deployment, if applicable)
- Git & GitHub (Version Control)

## 📌 Features
- ✅ Preprocesses email text (removes stopwords, tokenization, vectorization)
- ✅ Trained using multiple classifiers (Random Forest, Decision Tree, Naïve Bayes, etc.)
- ✅ Predicts spam vs. non-spam emails with high accuracy
- ✅ Serialized model using Pickle for easy deployment

# 📂 Project Structure
📁 Mail Classifier
│── 📄 app.py             # Main script to test the model  
│── 📄 train.py           # Model training script  
│── 📄 preprocess.py      # Text preprocessing module  
│── 📄 df.pkl             # Saved trained model  
│── 📄 cv.pkl             # CountVectorizer model  
│── 📄 dataset.csv        # Dataset used for training  
│── 📄 README.md          # Project documentation  

## 📊 Machine Learning Approach
1. Data Collection: Used a dataset containing spam and non-spam emails.
2. Text Preprocessing: Removed stopwords, tokenized, and converted text to numerical format using TF-IDF / CountVectorizer.
3. Model Training: Trained multiple classifiers (Random Forest, Decision Tree, Naïve Bayes) to identify the best-performing model.
4. Model Evaluation: Evaluated using accuracy, precision, recall, and F1-score.
5. Deployment: Saved the trained model using Pickle for quick predictions.

## 🚀 How to Run the Project
### 1️⃣ Clone the Repository  
```bash
git clone https://github.com/HarshBarnwal2004/Spam-Email-Classifier.git
cd Spam-Email-Classifier
```
### 2️⃣ Install Dependencies
```
pip install -r requirements.txt
```
### 3️⃣ Run the Classifier
```
python app.py
```
# Deployment on AWS EC2

* 1️⃣ Launch an AWS EC2 Instance
1. Go to AWS Console → EC2 → Launch Instance
2. Choose Ubuntu 20.04 / 22.04 as the OS
3. Select an instance type (e.g., t2.micro for free tier)
4. Configure Security Group:
 * Allow SSH (port 22)
 * Allow HTTP (port 80)
 * Allow Custom TCP Rule (port 5000 or 8000 for Flask)
5. Launch and connect via SSH

* 2️⃣ Connect to EC2 Instance
```
ssh -i your-key.pem ubuntu@your-ec2-public-ip
```
* 3️⃣ Install Required Packages
  ```
  sudo apt update && sudo apt upgrade -y
  sudo apt install python3-pip python3-venv nginx -y
  ```
* 4️⃣ Clone Your GitHub Repo & Install Dependencies
  ```
  git clone https://github.com/HarshBarnwal2004/Spam-Email-Classifier.git
  cd Spam-Email-Classifier
  python3 -m venv venv
  source venv/bin/activate
  pip install -r requirements.txt
  ```
* 5️⃣ Run Flask App with Gunicorn
  ```
  gunicorn --bind 0.0.0.0:5000 app:app
  ```
* 7️⃣ Set Up Nginx as a Reverse Proxy
  1. Open the Nginx config file
     ```
     sudo nano /etc/nginx/sites-available/spam_classifier
     ```
  2. Add the following configuration
     ```
     server {
     listen 80;
     server_name your-ec2-public-ip;

     location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       }
     }
     ```
  3. Enable the config and restart Nginx:
   ```
   sudo ln -s /etc/nginx/sites-available/spam_classifier /etc/nginx/sites-enabled
   sudo systemctl restart nginx
   ```
* 8️⃣ Keep the Flask App Running (Using Supervisor)
  To prevent the app from stopping when you disconnect:
  ```
  sudo apt install supervisor -y
  sudo nano /etc/supervisor/conf.d/spam_classifier.conf
  [program:spam_classifier]
  command=/home/ubuntu/Spam-Email-Classifier/venv/bin/gunicorn --bind 0.0.0.0:5000 app:app
  directory=/home/ubuntu/Spam-Email-Classifier
  autostart=true
  autorestart=true
  stderr_logfile=/var/log/spam_classifier.err.log
  stdout_logfile=/var/log/spam_classifier.out.log
  sudo supervisorctl reread
  sudo supervisorctl update
  sudo supervisorctl start spam_classifier
  ```
* 🚀 Done! Your API is Live
  ```
  http://your-ec2-public-ip/
  ```


   
   

