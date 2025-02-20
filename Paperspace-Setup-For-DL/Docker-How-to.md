# How I took the setup work and turned into docker image
After figuring out the build order, I learned how to make a docker file.
Make a new file and call it `Dockerfile` without any extension. I used Visual Studio 2022 to make it.

The Dockerfile looks like this:
```
# Use PyTorch with CUDA 12.1
FROM nvidia/cuda:12.4.0-devel-ubuntu22.04
# Official nvidia/cuda

# Install system dependencies and pip
RUN apt update && apt install -y \
    python3 python3-pip python3-dev \
    git wget curl nano && \
    rm -rf /var/lib/apt/lists/*

# Install pip for Python 3
RUN python3 -m pip install --upgrade pip

# Set the working directory
WORKDIR /workspace

# Ensure CUDA environment variables are set
ENV CUDA_HOME=/usr/local/cuda
ENV PATH="${CUDA_HOME}/bin:${PATH}"
ENV LD_LIBRARY_PATH="${CUDA_HOME}/lib64:${LD_LIBRARY_PATH}"

# Copy any local files you need
COPY requirements.txt .

# Install dependencies
RUN pip install --upgrade pip

#Install pytorch
RUN pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124

# Install packaging explicitly first
RUN pip install packaging==23.2

ENV CUDA_HOME=/usr/local/cuda

RUN ls /usr/local/cuda

#Install requirements
RUN pip install --no-cache-dir -r requirements.txt
RUN pip install "flash-attn>=2.7.3" --find-links "https://github.com/Dao-AILab/flash-attention/releases"

# Expose Jupyter port
EXPOSE 8888

# Start Jupyter with required settings
CMD ["jupyter", "notebook", "--no-browser", "--NotebookApp.trust_xheaders=True", \
     "--NotebookApp.disable_check_xsrf=False", "--NotebookApp.allow_remote_access=True", \
     "--NotebookApp.allow_origin='*'", "--ip=0.0.0.0", "--port=8888", "--allow-root"]
```
I pulled down the `requirements.txt` using `pip freeze > requirements.txt` and then removed unnecessary packages from the file.

After cleaning up the requirements, I took the specific package changes from my previous setup (e.g. `pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124`) and put it after the `RUN` command. I also set an `ENV` variable for cuda dir.

### Pushing to Dockerhub
I used the following commands to push to dockerhub:
> `sudo docker tag my_gradient_image jeffreysanglin/my_gradient_image:latest`
> 
> `sudo docker build -t my_gradient_image.`
> 
> `sudo docker build -t jeffreysanglin/my_gradient_image:latest .`
> 
> `sudo docker login`
> 
> `sudo docker push --disable-content-trust jeffreysanglin/my_gradient_image:latest`

