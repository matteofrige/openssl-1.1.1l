OpenSSL 1.1.1l Precompiled for AMD64 (Debian Docker Image)

This repository provides a precompiled version of OpenSSL 1.1.1l for AMD64, tailored for use in Debian-based Docker images. It is particularly useful when working with Microsoft SQL Server (MSSQL) and Python in environments where the default OpenSSL version is incompatible or outdated.

📦 What’s Included
	•	Precompiled OpenSSL 1.1.1l binary for AMD64 architecture.
	•	Prebuilt shared libraries (libssl.so, libcrypto.so).
	•	Example usage for integrating OpenSSL with Docker and Python applications requiring MSSQL.

🛠 Why This Repo?

Using MSSQL with Python in Dockerized Debian environments often requires a compatible OpenSSL version for secure connections. However:
	•	Debian’s OpenSSL version may not match the requirements.
	•	Compiling OpenSSL manually in Docker can be time-consuming and error-prone.

This repository provides a ready-to-use solution, saving time and simplifying integration.

📋 How to Use

1. Clone the Repository

git clone https://github.com/yourusername/openssl-1.1.1l-precompiled.git
cd openssl-1.1.1l-precompiled

2. Add OpenSSL to Your Docker Image

Copy the precompiled binaries to your Docker image. Here’s an example Dockerfile:

FROM --platform=linux/amd64 python:3.12-slim

ENV ACCEPT_EULA=Y

RUN wget https://github.com/matteofrige/openssl-1.1.1l/raw/refs/heads/main/openssl-1.1.1l.deb \
    dpkg -i openssl-1.1.1l.deb \
    rm openssl-1.1.1l.deb

# Update dynamic linker cache
RUN ldconfig

RUN curl -fsSL https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor -o /usr/share/keyrings/microsoft-prod.gpg \
    && curl https://packages.microsoft.com/config/debian/12/prod.list | tee /etc/apt/sources.list.d/mssql-release.list \
    && apt-get update \
    && apt-get install -y msodbcsql18

3. Verify the OpenSSL Version

After building your Docker image, check that the correct OpenSSL version is installed:

docker run --rm your-image-name openssl version

You should see:

OpenSSL 1.1.1l  24 Aug 2021

📝 Notes
	•	This repository assumes a Debian-based environment with amd64 architecture.
	•	For other architectures or OS versions, you may need to recompile OpenSSL.
	•	Ensure your Python libraries (e.g., pymssql) are compatible with the provided OpenSSL version.

📜 License

This repository follows the licensing terms of OpenSSL. Refer to the OpenSSL License for more details.

Let me know if you’d like to customize this further!
