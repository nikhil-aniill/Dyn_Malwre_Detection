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
   
4. ### Algorithm 
```
CuckooAPI
cuckoo_monitor(file)

  CuckooAPI(file)
  Report <- Cuckoo/Storage/Analyses/latest/reports
  Check the Hash and the score from info section
  Global Latest_score <- ["info"]["score"]



ChatMD client
client()

  connect(serverip,port)  
  create_user()
  create_chatroom()
  command<-user gives command
  if command has whohas 
     rb_to_png(file)
     output <- ML_Model(file)
  if command has getfile and output not benign
     Block file transfer
     Malware detected  
  elseif command has getfile and output is benign
     cuckoo_monitor(file)
      if latest_score greater than 1
         block file transfer
         malware detected
      else 
         Send the file



rb_to_png
rb_to_png(file,directory)

    open(filename,mode<-binary)
    Length <- filesize
    Width <-256
    Remainder= length%width
    Array <- unsigned char of grayscale bits with border of (length-remainder)
    v<-Convert array to 8 bit vector
    save(file,v)


ML_Model

Procedure Model()

Import the fast.ai libraries and torch
Fetch the data_bunch of malware families and benign files from their respective folders:
        Learner ← Initialize the resnet34 model with optimizer as accuracy
        Repeat for 10 epoch cycles for the data_bunch
            Learner ← Train and optimize on accuracy
        New_learning rate ← 10-2 and 10-3
        Repeat the for 10 epoch cycles for the New_learning rate
            Learner ← Train and optimize on accuracy 
        Pickle_file ← learn.export(trained.pkl)
            Return the Pickle_file
End procedure        



ML_API
Procedure API(input_image)
    input _image accepted on flask-server as form input
    Trained.pkl ← Fetch the pickle file from Model()
    Family ← Run Trained.pkl for the given input_image
    If the Family is benign
        Return "Benign"
    Else 
        Return "Malware and family is" + Family 
End procedure 
    

```

