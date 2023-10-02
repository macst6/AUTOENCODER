# AUTOENCODER

class Autoencoder

It respresents a deep autoencoder and its mirror components to construct encoder and decoder





COMPONENTS OF THE CLASS
----------------------------------------------------
----------------------------------------------------
----------------------------------------------------

Main componenets of the encoder class where we have
----------------------------------------------------
----------------------------------------------------
Input_shape ->(List)
Convolutions filters ->(List)
Convolutions kernels ->(List)
Convolutions strides ->(List)
Latent space ->(List)


Examples of the Inputs 
----------------------------------------------------
----------------------------------------------------
[28x28 pixels , 28x28 pixels , 1 channel(its because it looks like greyscale image) ]
[Number of filters : 2,4,8]
[These are the filters we are applying : 3x3,5x5,3x3]
[The data is being downsampled if the value increase in ascending : 1,2,2]
[Our bottle neck has 2 dimensions]



Main Objects that we want to create 
----------------------------------------------------
----------------------------------------------------

encoder = None
decoder = None
model = None





#Private attributes
------------------------------------------------------------
------------------------------------------------------------
number of convolution layers = length of convolution filters
shape before bottleneck = None -> (We will use this later so that we have an idea of the shape of the spectogram before it was compressed)
_build() -> (This method will be used to build the encoder)


FUNCTIONS TO BUILD THE COMPONENTS
----------------------------------------------------
----------------------------------------------------
----------------------------------------------------

Function of _build()
---------------------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------------------
In the build Method we have two sub methods which will be used to build our objects such as encoder and decoder which we described above



_build_encoder() 
---------------------------
---------------------------

Model
--------------------------------------------------------------------
We add the combination of Encoder input and Bottle Neck to the Model 
Encoder Input , Bottle Neck -> Model

encoder = Model(encoder_input, bottleneck, name="encoder")





_build_decoder()
-----------------
-----------------


Adding a conv layer like previous function
As we have a single channel for spectogram 
We are using the first layer which we ignored in previous process

This is for one channel
Adding this to incoming layers
Adding the final activation layer




Model
--------------------------------------------------------------------
We add the combination of Encoder input and Bottle Neck to the Model 
Decoder Input , Decoder Output -> Model

encoder = Model(encoder_input, bottleneck, name="encoder")




Function of _build_encoder() 
------------------------------------------------------------
Parts to build encoder are

# Encoder input
# Convolution Layer
# Bottle Neck


How to build
Encoder input -> Convolution Layer -> Bottle Neck


encoder_input = _add_encoder_input()
conv_layers = _add_conv_layers(encoder_input) #Convolution Layer = Encoder Input -> Convolution Layer
bottleneck =_add_bottleneck(conv_layers) # Bottleneck = Convolution Layer -> Bottleneck





Function of _add_encoder_input() 
------------------------------------------------------------
This Method will provide us with input of the encoder
returns the Input according to input shape
and we name it as encoder_input layer






Function of _add_conv_layer() -> This makes on layer which we will later on add with the encoder input
------------------------------------------------------------------------------------------------------
It adds a convolutional block to graph of layers consisting of
Conv2D { Filters , Kernel Size , Strides, padding , name }
ReLU
Batch Normalization




Function of _add_bottleneck() -> Here we will add a bottle neck to our input 
-------------------------------------------------------------------------------------------


We get the input as 
[2,7,Width=7,Height=7,Num_channels=32]
but we want 
[Width=7,Height=7,Num_channels=32] which is the shape before bottle neck


We want to flatten data to add to bottleneck which is a dense layer
Data -> Flatten -> Pass to Bottleneck


and we get the x that will later be used in the for loop where we add the convolution layers with the encoder input


Function of _add_decoder_input(): -> Here we will add decode the input
-------------------------------------------------------------------------------------------
We are converting shape of our input according to latent space dimension

Function of _add_dense_layer(): -> Here we will get a dense layer
-------------------------------------------------------------------------------------------
This function creates dense layers
[1,2,4]-> 8 : Multiplied all numbers to produce a product
# Passing the decoder input to a dense layer

Function of _add_reshape_layer(): -> Here we will reshape the layer
-------------------------------------------------------------------------------------------
We are reshaping the dense layer according to the shape before the bottle neck


Function of _add_conv_transpose_layer(x): -> for single convolution transpose of layer
-------------------------------------------------------------------------------------------

Layer nuumber = No of convolution layer - Index of the layers 



This makes the convolution transpose layer :
--------------------------------------------
we are taking Transpose of the Convolution layers
We want to take parameters as following 
Convolution filters , Covolution kernels and Convolution Strides should be go through layer index





We took the transpose of the updated layer

Then we applied the ReLU to the layer
Then we applied batch Normalization on the layer





Function of _add_conv_transpose_layers(x): -> Adds convolution transpose of the all the layer
-------------------------------------------------------------------------------------------

Adding transpose convolution block
We are passing it the x that is like the graph of all the layers 
x -> Encoder input at each layer index
we want to loop through all the convolution layers and stop at the first layer
In the for loop:
if range was : 0   ->   self._num_conv_layers [0,1,2] -> [2,1,0]
if range was : 1   ->   self._num_conv_layers [1,2]   -> [2,1]           (We want to use this format)
We took the transpose of x


And our function gives us the decoded input 


FUNCTIONS TO BUILD THE ENCODER
----------------------------------------------------
----------------------------------------------------
----------------------------------------------------

Function of _add_conv_layers() -> Here we are adding all the Layers together with the input 
-------------------------------------------------------------------------------------------
This Method will create all the with Convolution Blocks 


Inputs are 
#Encoder input     (Encoder Input -> Conv Layers)

We will create x here which is equal to encoder input 
Now we want to go layer by layer to all the steps
The loop will run until it gets all the layers in our architecture
In the loop we are:

x = on every index of Encoder Input -> adding conv layer
# adding encoder input at each layer index using _add_conv_layer and it will run until the last number of convolution Layer
# x will be like a graph of all the layers


Now This function will give us the x which is Encoder Input + Convolution Layer




FUNCTIONS TO BUILD THE DECODER
----------------------------------------------------
----------------------------------------------------
----------------------------------------------------


We are going to implement build decoder which will form our decoder
Provide the dense layer from decoder input : (Input of decoder -> Dense Layer)
Then we are reshaping the layer
Taking Convolution Transpose of Layer to transform it back
Then we are going to take decoder input
Then we are going to create a decoder Model
