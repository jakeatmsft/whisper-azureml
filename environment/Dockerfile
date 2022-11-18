FROM mcr.microsoft.com/azureml/aifx/stable-ubuntu2004-cu113-py38-torch1110:monthly.202210


# Install pip dependencies
RUN pip install 'ipykernel~=6.0' \
                'azureml-core==1.47.0' \
				'azureml-dataset-runtime==1.47.0' \
                'azureml-defaults==1.47.0' \
				'azure-ml==0.0.1' \
				'azure-ml-component==0.9.14' \
                'azureml-mlflow==1.47.0' \
                'azureml-telemetry==1.47.0' \
		        'azureml-contrib-services==1.47.0' \
                'torch-tb-profiler~=0.4.0' \
				'py-spy==0.3.12' \
		        'debugpy~=1.6.3'

RUN pip install \
        azure-ai-ml==0.1.0b5 \
        azureml-inference-server-http~=0.7.0 \
        inference-schema~=1.4.2.1 \
        MarkupSafe==2.0.1 \
	    regex \
	    pybind11

COPY requirements.txt ./

RUN pip install 'azureml-inference-server-http~=0.7.0'
RUN pip install -r requirements.txt
RUN pip install git+https://github.com/openai/whisper.git
RUN pip install jiwer


# Inference requirements
COPY --from=mcr.microsoft.com/azureml/o16n-base/python-assets:20220607.v1 /artifacts /var/
RUN /var/requirements/install_system_requirements.sh && \
    cp /var/configuration/rsyslog.conf /etc/rsyslog.conf && \
    cp /var/configuration/nginx.conf /etc/nginx/sites-available/app && \
    ln -sf /etc/nginx/sites-available/app /etc/nginx/sites-enabled/app && \
    rm -f /etc/nginx/sites-enabled/default
ENV SVDIR=/var/runit
ENV WORKER_TIMEOUT=400
EXPOSE 5001 8883 8888

# support Deepspeed launcher requirement of passwordless ssh login

RUN apt-get update
RUN apt-get install -y ffmpeg
RUN apt-get install -y openssh-server openssh-client
RUN mkdir -p /root/.ssh
RUN mkdir /var/run/sshd
RUN ssh-keygen -t rsa -f /root/.ssh/id_rsa
RUN sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\\s*required\\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN chmod 700 /root/.ssh/
RUN touch /root/.ssh/config;echo -e "Port 1043\n StrictHostKeyChecking no\n  UserKnownHostsFile=/dev/null" > /root/.ssh/config
RUN echo "Port 1043" >> /etc/ssh/sshd_config
RUN chmod 600 /root/.ssh/config
RUN touch /root/.ssh/authorized_keys && chmod 600 /root/.ssh/authorized_keys
RUN cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
EXPOSE 1043 1043
CMD ["/usr/sbin/sshd", "-D"]
