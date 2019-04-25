# Face Detection<a name="EN-US_TOPIC_0167099111"></a>

## Principle<a name="en-us_topic_0167089636_section2448105318293"></a>

Developers can deploy the application on the Atlas 200 DK to collect camera data in real time and predict facial information in the video.  [Figure 1](#en-us_topic_0167089636_fig261318405454)  shows the process.

**Figure  1**  Implementation process of the face detection sample<a name="en-us_topic_0167089636_fig261318405454"></a>  
![](doc/source/img/implementation-process-of-the-face-detection-sample.png "implementation-process-of-the-face-detection-sample")

## Prerequisites<a name="en-us_topic_0167089636_section412314183119"></a>

Before using an open source application, ensure that:

-   Mind Studio has been installed. For details, see  [Mind Studio Installation Guide](https://www.huawei.com/minisite/ascend/en/filedetail_1.html).
-   The Atlas 200 DK developer board has been connected to Mind Studio, the cross compiler has been installed, the SD card has been prepared, and basic information has been configured. For details, see  [Atlas
200 DK User Guide](https://www.huawei.com/minisite/ascend/en/filedetail_2.html).
-   The Atlas 200 DK developer board has been connected to cameras.

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >The Raspberry Pi v1.3 and v2.1 cameras are supported, which need to be connected to two 22-pin Pi Zero connectors.  
    >The camera needs to be connected when the developer board is powered off.  

-   Google Chrome 67.0.3396.87 or later is available.

## Software Preparation<a name="en-us_topic_0167089636_section177411912193214"></a>

Before running the application, obtain the source code package and configure the environment as follows.

1.  Obtain the source code package.

    Download all the code in the sample-facedetection repository at  [https://github.com/Ascend/sample-facedetection](https://github.com/Ascend/sample-facedetection)  to any directory on Ubuntu Server where Mind Studio is located as the Mind Studio installation user, for example,  _/home/ascend/sample-facedetection/_.

2.  Log in to Ubuntu Server where Mind Studio is located as the Mind Studio installation user and set the environment variable** DDK\_HOME**.

    **vim \~/.bashrc**

    Run the following commands to add the environment variables  **DDK\_HOME**  and  **LD\_LIBRARY\_PATH**  to the last line:

    **export DDK\_HOME=/home/XXX/tools/che/ddk/ddk**

    **export LD\_LIBRARY\_PATH=$DDK\_HOME/uihost/lib**

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >-   **XXX**  indicates the Mind Studio installation user, and  **/home/XXX/tools**  indicates the default installation path of the DDK.  
    >-   If the environment variables have been added, skip this step.  

    Enter** :wq! **to save and exit.

    Run the following command for the environment variable to take effect:

    **source \~/.bashrc**


## Deployment<a name="en-us_topic_0167089636_section15718149133616"></a>

1.  <a name="en-us_topic_0167089636_li206239341242"></a>If the Ubuntu system where Mind Studio is located is not connected to the network, prepare the model files by referring to this step. If the Ubuntu system where Mind Studio is located is connected to the network, the deployment script automatically downloads the model files, and you can ignore this operation.

    Download the model file  **face\_detection.om**  used by the face detection application by referring to  [Downloading Network Model](#en-us_topic_0167089636_section193081336153717), and place the .om file in the root directory of the face detection application code, for example,  _home/ascend/sample-facedetection_.

2.  Access the root directory where the face detection application code is located as the Mind Studio installation user, for example,  _**/home/ascend/sample-facedetection**_.
3.  Run the deployment script to prepare the project environment, including compiling and deploying the ascenddk public library, downloading the network model, and configuring Presenter Server.

    **bash deploy.sh **_host\_ip_ _model\_mode_

    -   _host\_ip_: For the Atlas 200 DK developer board, this parameter indicates the IP address of the developer board.
    -   _model\_mode_  indicates the deployment mode of the model file. The value can be  **local**  or** internet**. The default setting is  **internet**.
        -   **local**: Indicates the local deployment mode. If the Ubuntu system where Mind Studio is located is not connected to the network, use the local mode. In this case, download the network model file to the root directory of the application code, that is,  _**sample-facedetection**_, by referring to  [1](#en-us_topic_0167089636_li206239341242).
        -   **internet**: Indicates the online deployment mode. If the Ubuntu system where Mind Studio is located is connected to the network, use the Internet mode. In this case, download the model file online.


    Example command:

    **bash deploy.sh 192.168.1.2 internet**

    When the message  **Please choose one to show the presenter in browser\(default: 127.0.0.1\):**  is displayed, enter the IP address used for accessing the Presenter Server service in the browser. Generally, the IP address is the IP address for accessing the Mind Studio service.

    Select the IP address used by the browser to access the Presenter Server service in  **Current environment valid ip list**, as shown in  [Figure 2](#en-us_topic_0167089636_fig184321447181017).

    **Figure  2**  Project deployment<a name="en-us_topic_0167089636_fig184321447181017"></a>  
    ![](doc/source/img/project-deployment.png "project-deployment")

4.  Start Presenter Server.

    Run the following command to start the Presenter Server program of the face detection application in the background:

    **python3 presenterserver/presenter\_server.py --app face\_detection &**

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >**presenter\_server.py**  is located in the  **presenterserver**  in the current directory. You can run the  **python3 presenter\_server.py -h**  or  **python3 presenter\_server.py --help**  command in this directory to view the usage method of  **presenter\_server.py**.  

    [Figure 3](#en-us_topic_0167089636_fig69531305324)  shows that the presenter\_server service is started successfully.

    **Figure  3**  Starting the Presenter Server process<a name="en-us_topic_0167089636_fig69531305324"></a>  
    ![](doc/source/img/starting-the-presenter-server-process.png "starting-the-presenter-server-process")

    Use the URL shown in the preceding figure to log in to Presenter Server \(only the Chrome browser is supported\). The IP address is that entered in  [Figure 4](#en-us_topic_0167089636_fig64391558352)  and the default port number is  **7007**. The following figure indicates that Presenter Server is started successfully.

    **Figure  4**  Home page<a name="en-us_topic_0167089636_fig64391558352"></a>  
    ![](doc/source/img/home-page.png "home-page")


## Running<a name="en-us_topic_0167089636_section10271726154420"></a>

1.  Run the face detection application.

    Run the following command in the  **sample-facedetection**  directory \(for example,** /home/ascend/sample-facedetection**\) to start the face detection application:

    **bash run\_facedetectionapp.sh** _host\_ip_ _presenter\_view\_app\_name camera\_channel\_name_  &

    -   _host\_ip_: For the Atlas 200 DK developer board, this parameter indicates the IP address of the developer board.
    -   _presenter\_view\_app\_name_: Indicates  **View Name**  displayed on the Presenter Server page, which is user-defined.
    -   _camera\_channel\_name_: Indicates the channel to which a camera belongs. The value can be  **Channel-1**  or  **Channel-2**. For details, see  [Viewing the Channel to Which a Camera Belongs](#en-us_topic_0167089636_section29013915461).

    Example command:

    **bash run\_facedetectionapp.sh 192.168.1.2 video Channel-1 &**

2.  Use the URL that is displayed when you start the Presenter Server service to log in to the Presenter Server website \(only the Chrome browser is supported\). For details, see  [5](en-us_topic_0165443743.md#li499911453439).

    Wait for Presenter Agent to transmit data to the server. Click  **Refresh**. When there is data, the icon in the  **Status**  column for the corresponding channel changes to green, as shown in  [Figure 5](#en-us_topic_0167089636_fig113691556202312).

    **Figure  5**  Presenter Server page<a name="en-us_topic_0167089636_fig113691556202312"></a>  
    ![](doc/source/img/presenter-server-page.png "presenter-server-page")

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >-   The Presenter Server of the face detection application supports a maximum of 10 channels at the same time \(each  _presenter\_view\_app\_name_  parameter corresponds to a channel\).  
    >-   Due to hardware limitations, the maximum frame rate supported by each channel is 20fps,  a lower frame rate is automatically used when the network bandwidth is low.  

3.  Click  **image**  or  **video**  in the  **View Name**  column and view the result. The confidence of the detected face is marked.

## Follow-up Operations<a name="en-us_topic_0167089636_section1092612277429"></a>

-   **Stopping the Face Detection Application**

    The face detection application is running continually after being executed. To stop it, perform the following operation:

    Run the following command in the  _**/home/ascend/sample-facedetection**_  directory as the Mind Studio installation user:

    **bash stop\_facedetectionapp.sh** _host\_ip_

    _host\_ip_: For the Atlas 200 DK developer board, this parameter indicates the IP address of the developer board.For the Atlas 300 PCIe card, this parameter indicates the IP address of the PCIe card host.

    Example command:

    **bash stop\_facedetectionapp.sh 192.168.1.2**

-   **Stopping the Presenter Server Service**

    The Presenter Server service is always in the running state after being started. To stop the Presenter Server service of the face detection application, perform the following operations:

    Run the following command to check the process of the Presenter Server service corresponding to the face detection application as the Mind Studio installation user:

    **ps -ef | grep presenter | grep face\_detection**

    ```
    ascend@ascend-HP-ProDesk-600-G4-PCI-MT:~/sample-facedetection$ ps -ef | grep presenter | grep face_detection
    ascend    7701  1615  0 14:21 pts/8    00:00:00 python3 presenterserver/presenter_server.py --app face_detection
    ```

    In the preceding information,  _7701_  indicates the process ID of the Presenter Server service corresponding to the face detection application.

    To stop the service, run the following command:

    **kill -9** _7001_


## Downloading Network Model<a name="en-us_topic_0167089636_section193081336153717"></a>

The models used in the Ascend open source applications are converted models that adapt to the Ascend 310 chipset. For details about how to download this kind of models and the original network models of face detection application, see  [Table 1](#en-us_topic_0167089636_table0531392153). If you have a better model solution, you are welcome to share it at  [https://github.com/Ascend/models](https://github.com/Ascend/models).

**Table  1**  Models used in face detection applications

<a name="en-us_topic_0167089636_table0531392153"></a>
<table><thead align="left"><tr id="en-us_topic_0167089636_row1154103991514"><th class="cellrowborder" valign="top" width="15.841584158415841%" id="mcps1.2.5.1.1"><p id="en-us_topic_0167089636_p195418397155"><a name="en-us_topic_0167089636_p195418397155"></a><a name="en-us_topic_0167089636_p195418397155"></a>Model Name</p>
</th>
<th class="cellrowborder" valign="top" width="21.782178217821784%" id="mcps1.2.5.1.2"><p id="en-us_topic_0167089636_p1054539151519"><a name="en-us_topic_0167089636_p1054539151519"></a><a name="en-us_topic_0167089636_p1054539151519"></a>Description</p>
</th>
<th class="cellrowborder" valign="top" width="28.71287128712871%" id="mcps1.2.5.1.3"><p id="en-us_topic_0167089636_p387083117108"><a name="en-us_topic_0167089636_p387083117108"></a><a name="en-us_topic_0167089636_p387083117108"></a>Model Download Path</p>
</th>
<th class="cellrowborder" valign="top" width="33.663366336633665%" id="mcps1.2.5.1.4"><p id="en-us_topic_0167089636_p35412397154"><a name="en-us_topic_0167089636_p35412397154"></a><a name="en-us_topic_0167089636_p35412397154"></a>Original Network Download Address</p>
</th>
</tr>
</thead>
<tbody><tr id="en-us_topic_0167089636_row65414393159"><td class="cellrowborder" valign="top" width="15.841584158415841%" headers="mcps1.2.5.1.1 "><p id="en-us_topic_0167089636_p17544398153"><a name="en-us_topic_0167089636_p17544398153"></a><a name="en-us_topic_0167089636_p17544398153"></a>Network model for face detection</p>
<p id="en-us_topic_0167089636_p84114461512"><a name="en-us_topic_0167089636_p84114461512"></a><a name="en-us_topic_0167089636_p84114461512"></a>(<strong id="en-us_topic_0167089636_b41111030191911"><a name="en-us_topic_0167089636_b41111030191911"></a><a name="en-us_topic_0167089636_b41111030191911"></a>face_detection.om</strong>)</p>
</td>
<td class="cellrowborder" valign="top" width="21.782178217821784%" headers="mcps1.2.5.1.2 "><p id="en-us_topic_0167089636_p169011731015"><a name="en-us_topic_0167089636_p169011731015"></a><a name="en-us_topic_0167089636_p169011731015"></a>This model is used in the <strong id="en-us_topic_0167089636_b15967719155417"><a name="en-us_topic_0167089636_b15967719155417"></a><a name="en-us_topic_0167089636_b15967719155417"></a>face detection</strong> application.</p>
<p id="en-us_topic_0167089636_p1372429181516"><a name="en-us_topic_0167089636_p1372429181516"></a><a name="en-us_topic_0167089636_p1372429181516"></a>It is a network model converted from ResNet0-SSD300 model based on Caffe.</p>
</td>
<td class="cellrowborder" valign="top" width="28.71287128712871%" headers="mcps1.2.5.1.3 "><p id="en-us_topic_0167089636_p1569513572242"><a name="en-us_topic_0167089636_p1569513572242"></a><a name="en-us_topic_0167089636_p1569513572242"></a>Download the model from the <strong id="en-us_topic_0167089636_b028612482311"><a name="en-us_topic_0167089636_b028612482311"></a><a name="en-us_topic_0167089636_b028612482311"></a>computer_vision/object_detect/face_detection</strong> directory in the <a href="https://github.com/Ascend/models/" target="_blank" rel="noopener noreferrer">https://github.com/Ascend/models/</a> repository.</p>
<p id="en-us_topic_0167089636_p1787118315101"><a name="en-us_topic_0167089636_p1787118315101"></a><a name="en-us_topic_0167089636_p1787118315101"></a>For the version description, see the <strong id="en-us_topic_0167089636_b1012219832511"><a name="en-us_topic_0167089636_b1012219832511"></a><a name="en-us_topic_0167089636_b1012219832511"></a>README.md</strong> file in the current directory.</p>
</td>
<td class="cellrowborder" valign="top" width="33.663366336633665%" headers="mcps1.2.5.1.4 "><p id="en-us_topic_0167089636_p1785381617217"><a name="en-us_topic_0167089636_p1785381617217"></a><a name="en-us_topic_0167089636_p1785381617217"></a>For details, see the <strong id="en-us_topic_0167089636_b1423252411265"><a name="en-us_topic_0167089636_b1423252411265"></a><a name="en-us_topic_0167089636_b1423252411265"></a>README.md</strong> file of the <strong id="en-us_topic_0167089636_b688544332614"><a name="en-us_topic_0167089636_b688544332614"></a><a name="en-us_topic_0167089636_b688544332614"></a>computer_vision/object_detect/face_detection</strong> directory in the <a href="https://github.com/Ascend/models/" target="_blank" rel="noopener noreferrer">https://github.com/Ascend/models/</a> repository.</p>
<p id="en-us_topic_0167089636_p1314312124919"><a name="en-us_topic_0167089636_p1314312124919"></a><a name="en-us_topic_0167089636_p1314312124919"></a><strong id="en-us_topic_0167089636_b1365251225519"><a name="en-us_topic_0167089636_b1365251225519"></a><a name="en-us_topic_0167089636_b1365251225519"></a>Precautions during model conversion:</strong></p>
<p id="en-us_topic_0167089636_p53116302463"><a name="en-us_topic_0167089636_p53116302463"></a><a name="en-us_topic_0167089636_p53116302463"></a>During the conversion, a message is displayed indicating that the conversion fails. You only need to select <strong id="en-us_topic_0167089636_b55978299556"><a name="en-us_topic_0167089636_b55978299556"></a><a name="en-us_topic_0167089636_b55978299556"></a>SSDDetectionOutput </strong>from the drop-down list box for the last layer and click <strong id="en-us_topic_0167089636_b15597182918551"><a name="en-us_topic_0167089636_b15597182918551"></a><a name="en-us_topic_0167089636_b15597182918551"></a>Retry</strong>.</p>
<p id="en-us_topic_0167089636_p109405475158"><a name="en-us_topic_0167089636_p109405475158"></a><a name="en-us_topic_0167089636_p109405475158"></a><a name="en-us_topic_0167089636_image13957135893610"></a><a name="en-us_topic_0167089636_image13957135893610"></a><span><img id="en-us_topic_0167089636_image13957135893610" src="doc/source/img/en-us_image_0167095810.png"></span></p>
<p id="en-us_topic_0167089636_p179225194910"><a name="en-us_topic_0167089636_p179225194910"></a><a name="en-us_topic_0167089636_p179225194910"></a></p>
</td>
</tr>
</tbody>
</table>

## Viewing the Channel to Which a Camera Belongs<a name="en-us_topic_0167089636_section29013915461"></a>

To query the channel to which a camera belongs, perform the following steps:

1.  On the Mind Studio main menu, choose  **Tools \> Atlas DK Configuration**. The  **Atlas DK Configuration**  interface is displayed, as shown in  [Figure 6](#en-us_topic_0167089636_fig1278807124918).

    **Figure  6**  Atlas 200 DK Configuration dialog box<a name="en-us_topic_0167089636_fig1278807124918"></a>  
    ![](doc/source/img/atlas-200-dk-configuration-dialog-box.png "atlas-200-dk-configuration-dialog-box")

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >If no Atlas DK connection exists on the configuration page, click  **Add**  to add a connection. For details, see the  _Atlas 200 Developer Kit User Guide_.  

2.  Select the Atlas DK connection in use and click  **Connect**  to check the connection status of the Atlas DK, as shown in  [Figure 7](#en-us_topic_0167089636_fig1178814714917).

    **Figure  7**  Atlas 200 DK status dialog box<a name="en-us_topic_0167089636_fig1178814714917"></a>  
    ![](doc/source/img/atlas-200-dk-status-dialog-box.png "atlas-200-dk-status-dialog-box")

    In the preceding figure,  **Camera2**  is set to  **Online**, indicating that the channel to which that the current camera belongs is  **Channel-2**.


