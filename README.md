
# HASS-Deepstack-object
[Home Assistant](https://www.home-assistant.io/) custom components for using Deepstack object detection. [Deepstack](https://python.deepstack.cc/) is a service which runs in a docker container and exposes deep-learning models via a REST API. Deepstack [object detection](https://python.deepstack.cc/object-detection) can identify 80 different kinds of objects, including people (`person`) and animals. There is no cost for using Deepstack, although you will need a machine with 8 GB RAM. On your machine with docker, pull the latest image (approx. 2GB):

```
docker pull deepquestai/deepstack

OR 

docker pull deepquestai/deepstack:noavx
```
**Recommended OS** Deepstack docker containers are optimised for Linux or Windows 10 Pro. Mac and regular windows users my experience performance issues. [You can also run deepstack on a Raspberry pi](https://python.deepstack.cc/raspberry-pi) if you own an Intel NCS (Movidius) stick (approx $70).

**GPU users** Note that if your machine has an Nvidia GPU you can get a 5 x 20 times performance boost by using the GPU.

**Legacy machine users** If you are using a machine that doesn't support avx or you are having issues with making requests, Deepstack has a specific build for these systems. Use `deepquestai/deepstack:noavx` instead of `deepquestai/deepstack` when you are installing or running Deepstack. I expect many users will be using noavx mode so I will use it in the examples below.

## Activating the Deepstack API
Before you get started, you will need to activate the Deepstack API. First, go to www.deepstack.cc and sign up for an account. Choose the basic plan which will give us unlimited access for one installation. You will then see an activation key in your portal.

On your machine with docker, run Deepstack (noavx mode) without any recognition so you can activate the API on port `5000`:
```
docker run -v localstorage:/datastore -p 5000:5000 deepquestai/deepstack:noavx
```

Now go to http://YOUR_SERVER_IP_ADDRESS:5000/ on another computer or the same one running Deepstack. Input your activation key from your portal into the text box below "Enter New Activation Key" and press enter. Now stop your docker container, and restart and run Deepstack (noavx mode) with the object detection service active on port `5000`:
```
docker run -e VISION-DETECTION=True -e API-KEY="Mysecretkey" -v localstorage:/datastore -p 5000:5000 --name deepstack -d deepquestai/deepstack:noavx
```

## Usage of this component

Check official documentation [here](https://github.com/robmarkcole/HASS-Deepstack-object).

## Changes from robin version

- Entity's state value is 0 when no prediction/detection
- Timestamp option will store images in a subfolder named **source.name**
- Changed DATETIME_FORMAT used when timestamp option enabled to have filename compatbile with Windows system also (avoir XXX~.jpg when displayed in the Explorer)
