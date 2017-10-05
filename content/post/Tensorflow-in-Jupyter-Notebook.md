+++
date = "2017-10-05T08:34:17+01:00"
title = "Tensorflow in Jupyter Notebook"
menu="main"
+++
Jupyter Notebook is a wonderful tool for machine learning in the Python world. Yes, I know, you can use other languages in Python, but I've never tried it.

Anyway, it was time for me to start looking at Tensorflow and obviuosly I wanted to use it in Jupyter. This turned out to be a bit of a struggle and it took a few hours of Googling before I got it to work.

## Installing Tensorflow
Tensorflow provides [good instructions](https://www.tensorflow.org/install/install_mac) to install the actual tool. No problems here. There are several different ways to install TF and I decided to run in through Virtualenv on my Mac.  Just to be clear on what I did I'll quickly run through the steps.
````
$ sudo pip3 install --upgrade virtualenv
$ virtualenv --system-site-packages -p python3 ~/tensorflow
$ source ~/tensorflow/bin/activate
````

"~/tensorflow" is the name of the virtual environment and can be named whatever you want it to be.

Now the virtual environment should start and the prompt should change to `(tensorflow)$ `. Time to actually install TF!
````
(tensorflow)$ easy_install -U pip3
(tensorflow)$ pip3 install --upgrade tensorflow
````

That's it. Just to check the installation I copied the script in the instructions and ran it;
````
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
````

## Jupyter
The problem came when I opend Jupyter and tried to run the same script there. I was met by this error message:<br>
![TF  Error](/tf-jupyter-error.png)<br>
. The problem is that Tensorflow is running under Virtualenv and Jupyter Notebook is not, so they are completely unaware of each other. 

As I mentioned, it took a bit of searching before I found [this](https://www.youtube.com/watch?v=jv8gQd4g0Og) video, which in turn reference [this](http://fosshelp.blogspot.com.es/2017/08/how-to-add-python-virtualenv-to-ipython.html) blog post. Anyway, with virtual environment active I did this:
````
(tensorflow)$ pip3 install ipykernel
(tensorflow)$ python -m ipykernel install --user --name=tensorflow
````

And with that in place I could start Jupyter and create a new notebook. What to keep in mind though is that you can't pick a regular Python nothebook, but must choose the virtual environment.<br>
![TF Jupyter](/tf-jupyter.png).

## Bonus starting virtual environment
To start the virtual environment you must type in `source ~/tensorflow/bin/activate` every time. Seems a bit tedious to me, so I put this in my .bash_profile:
````
tf(){
 73   source ~/tensorflow/bin/activate
 74 }
 ````

 All I have to do now is run `$ tf` and the virtual environment will kick in. And in case you're wondering you kill Virtualenv with `$ deactivate`.




