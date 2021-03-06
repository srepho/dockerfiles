# Start with CUDA base image
FROM kaixhin/cuda
MAINTAINER Kai Arulkumaran <design@kaixhin.com>

# Install OpenSSH
RUN apt-get update && apt-get install -y openssh-server
# Set password
COPY password.txt .
RUN mkdir /var/run/sshd && \
  echo "root:`cat password.txt`" | chpasswd && \
  # Allow root login with password
  sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
  # Prevent user being kicked off after login
  sed -i 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' /etc/pam.d/sshd && \
  # Clean up
  rm password.txt
# Expose SSH port
EXPOSE 22
# Add CUDA back to path during SSH
RUN echo "export PATH=$PATH:/usr/local/cuda/bin" >> /etc/profile

# Run SSH server without detaching
CMD ["/usr/sbin/sshd", "-D"]
