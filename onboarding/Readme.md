## Step 1: Transfer learn a sparse model onto our target use case

See `transfer_learn.ipynb`, which demonstrates how to do this. Note: this notebook was run on Google Colab.

## Step 2: Export the final Pytorch model to ONNX.

We do this at the end of `transfer_learn.ipynb`, and renamed the file `model.onnx`, which is in the `onboarding` directory.

## Step 3: Build Docker Image for server

Run `docker build -t deepsparse-docker-server .` in the `onboarding` directory

This docker container loads the dependencies for the `deepsparse engine` and `deepsparse server` and prepares to run a script called `run.sh` which launches the server with the model `model.onnx`.

## Step 4: Run Docker Container with the server

Run `docker run -p 5551:5551 deepsparse-docker-server`

NOTE: the `-p` argument forwards the `5551` port from the host to the container. The server is listening on port `5551`, so we could have the localhost accept an aribtrary port but we must forward it to `5551`

## Step 5: Perform inference

See `send_server_requests.ipynb`, which demonstreates how to do this. We simply load the validation set from the `Imagenette` database and send requests over `HTTP` to the server running in the docker container. As you can see, the server expects denomalized images (i.e. it runs the normalization preprocessing on the server side) - I originally did this twice (once on client, once on server side) and was getting poor results. However, we can see that we are getting 95% accuracy on this validation set which is in line with the training process and therefore everything seems to be working correctly.
