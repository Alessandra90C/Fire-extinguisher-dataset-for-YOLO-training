# Fire-extinguisher-dataset-for-YOLO-training
In order to train the network it is necessary to have a training environment.
The following ones are all the necessary requirements [AlexeyAB, 2019]:
1. Windows or Linux;
2. CMake >= 3.8 for modern CUDA support;
3. CUDA;
4. OpenCV >= 2.4;
5. cuDNN >= 7.0;
6. GPU with CC >= 3.0;
7. on Linux GCC or Clang, on Windows MSVC 2015/2017/2019.
Since this project involves the use of YOLO neural network it has been decided
to use the training platform advised by the developer of the network itself:
Darknet-19 [Shinde et al., 2018]. Darknet is an open source neural network
framework written in C and CUDA. It supports CPU and GPU computation
[Redmon, 2018].
In order to install Darknet it is necessary to set the proper environment. This
consists mainly in the installing of a development system and the aforementioned
dependancies [Redmon, 2016]. The development system is Visual Studio
installed with its default options. The dependencies are CUDA, cuDNN and
OpenCV.
Starting from CUDA, the version installed is the 9.1. This installation requires
also the installation of the NVIDIA Graphics Drivers if not yet on the pc.
The second installation to be done is cuDNN version 7.0.
Finally it follows the installation of OpenCV 3.4.0.
After having done all this installations Darnet needs to be compiled with the
following procedure:
1. Start Microsoft Visual Studio
2. Open the darknet.sln
3. set x64 and Release
4. Include cudnn.lib in your Visual Studio project
5. Build > Build darknet.
At this moment the darknet.exe is generated inside the folder.
Finally darknet needs to be prepared for using OpenCV, CUDA and cuDNN.
The bin file has to be placed in the same folder of darknet.exe. Bin
and include folders have to be inserted also in CUDA folder if they are not
already there. Finally a new Windows variable cudnn has to be created.

Bounding the images
The creation of the dataset involves also labelling all the images. Labeling defines
the approach of marking all regions of interest in a set of pictures and
defining the type of marked region [Braun et al., 2019]. Creating the label involves
both the design of the bounding box around the object to recognize and
attaching the correct label to it. This operation could be performed in many
different ways both manually and with the aid of software tools.
In the case performed in this thesis we worked using a tool that supports the
manual drawing of the bounding box and which gives the possibility of adding
a label to every single box according to the category of the object involved in
the recognition (Figure 3.21) [VoTT, 2019].
This tool gives the possibility to choose the right output format according to
the kind of network chosen. The output for the YOLO network is a .txt file,
with the coordinates of the boxes and the label attached to them.

The final task to complete the dataset is the definition of the train and test
sets of images. The train set is defined by a .txt file listing all the images that
will be used for training. On the other hand the test list of images, also defined
by a .txt file, will be used after the training phase to verify the performances of
the network.

After having created the dataset the training session can start.
There are some files to set before starting the process:
1. the .cfg file of the network chosen
2. the .data file
3. the .names file
4. the .weights file
As far as the .cfg file is concerned some parameters have to been taken into
consideration for training a customized network. The parameters to check are
[Tijtgat, 2017] [AlexeyAB, 2019]:
1. batch = 64, this means we will be using 64 images for every training step;
2. subdivision = 8, the batch will be divided by 8 to decrease GPU VRAM
requirements. If you have a powerful GPU with loads of VRAM, this
number can be decreased, or batch could be increased. The training step
will throw a CUDA out of memory error so you can adjust accordingly;
1. classes = 1, the number of categories we want to detect;
2. filters = (classes + 5)*5.
The .data file is the file that the software reads to train the network. It contains
all the paths to the other necessary files for the training process.
The classes parameter is the number of different objects for the training
process, the train is the path of the train list of images, the valid is the path
for the test list of images, names is the path for the .names file and finally the
backup define the name of the backup folder where all the weights created will
be saved.
The .names file is the file that contains the name of the tag inside
the images of the dataset. The tags are identified by the row number inside the
file.
The weights have to been chosen according to the network that one wants
to train. Choosing the weight file means that the network used is a pre-trained
network with a general dataset (CoCo, PascalVoc or others). It would be possible
also not to choose any weights file and in that case the network will be
trained from scratch and it will not profit from the transfer learning process.
The training command line can be sent from Windows Power Shell, a commandline
shell with associated scripting language. To be in power of letting the train
start the command has to be sent inside the folder where the executable of
Darkent is located.
The -map > log.txt command is an output request so as to have the trend of
the mean average precision and the loss mapped on a chart.
During the training of the network on one hand it is possible to see in a chart the mean average precision (mAP) increasing in value. On the other
hand, there is the calculation of the loss function by means of Equation 1. The
loss function only penalizes classification error if an object is present in that grid
cell. It also penalizes bounding box coordinate error only if that predictor is
“responsible” for the ground truth box (i.e. has the highest IOU of any predictor
in that grid cell).
The output of the training process therefore are:
1. the chart with the mAP progress in red and the Avg progress in blue;
2. the log file with all the operations executed to train the network, it contains the report of all the epoch, with the avg and mAP
values;
3. the backup folder which contains the weights of the trained network saved
at predefined stages (1000 epochs this case).
