FROM python:3.8-slim

ENV VIRTUAL_ENV=/opt/venv
RUN python3 -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"
RUN pip3 install "deepsparse[server]"
RUN pip3 install torch==1.9.1 torchvision==0.10.1

WORKDIR /endpoint
COPY model.onnx model.onnx
COPY run.sh run.sh
RUN chmod +x run.sh

ENV port 5551
EXPOSE ${port}

CMD ./run.sh model.onnx ${port}