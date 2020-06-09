# Dynamic Malware Detection ML Model 
This project is an intrusion detection model which comprise of two separate components, first being the cuckoo sandbox and second the malware visualization component on the files sent through a chat server. The machine learning model will successfully tell which malware family does a particular file belong to. Both the benign and malicious files are converted to an 8 bit vector to generate their respective greyscale images. 

The main theory behind choosing this approach is that the same family of malware use same binary pattern. Processing text is comparatively slower than processing images. Hence, R-CNN is used. It not only is faster in this scenario but it also overcomes the issues of vanishing gradient which are encountered while using traditional CNN model. 

## Getting started
1. ### Installing the Malware detection API
   Docker should be installed according to the Linux distribution. 
```
   git clone https://github.com/nikhil-aniill/Dyn_Malwre_Detection.git
   cd API/fastai-v3-master
   mkvirtualenv --python=python3.8 myvenv
   pip install -r requirements.txt
   docker build -t fastai-v3 . && docker run --rm -it -p 5000:5000 fastai-v3(according to name in docker-compose.yml)
```
   You can check if the Docker is running by typing localhost:5000 into the URL bar.
2. ### Installing the [ChatMD](https://github.com/nikhil-aniill/chat_server) 
```
   git clone https://github.com/nikhil-aniill/chat_server.git
   cd chat_server/code
```
     Follow the README file on [here](https://github.com/nikhil-aniill/chat_server) to run ChatMD
3. ### Installing Cuckoo Sandbox 
   Refer to the link step by step to install [Cuckoo](https://medium.com/@warunikaamali/cuckoo-sandbox-installation-guide-d7a09bd4ee1f). 
   The analyses folder which consists of the reports of the sandboxed files should be updated in client.py of ChatMD.


