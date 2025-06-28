# 🔐 SECURE-MSG

**SECURE-MSG** is a secure messaging system that enables confidential communication between users using hybrid cryptography. This project integrates post-quantum key exchange, symmetric encryption, and digital signatures, ensuring message authenticity, integrity, and privacy. A central server facilitates signature validation, message distribution, and secure key exchange management.

---

## 📌 Project Goals

- Ensure confidentiality of messages through AES encryption.
- Guarantee authenticity and integrity using DSA digital signatures.
- Implement post-quantum key exchange via Kyber.
- Encrypt and securely store users’ private keys locally.
- Reuse shared keys between known users to optimize message transmission.

---

## 🔑 Key System Overview

### 🖥️ Server (DSA)

- **Private Key**: Used to sign all content uploaded to the server.
- **Public Key**: Used to verify all content read from the server.

### 👤 User

- **Kyber Key Pair**:
  - **Public Key**: Shared with other users to derive a shared AES key for communication.
  - **Private Key**: Stored locally and encrypted; used to derive the shared AES key from the secret received.
  
- **AES Keys**:
  - **Personal AES Key**: Derived from the user's password, used to encrypt/decrypt local content and protect the Kyber private key.
  - **Shared AES Keys**: Dynamically generated using another user’s Kyber public key, used for secure message exchange between users.

---

## ⚙️ How It Works

### 🔐 Encryption Workflow

1. **Initialization**:
   - The server generates a DSA key pair and creates two directories:
     - `general/`: Public data accessible by all users.
     - `local/<username>/`: Private directory for each user.
   - A user logs in using their username and password.
   - Upon login, the user is assigned:
     - A Kyber key pair.
     - A personal AES key derived from their password.
   - The Kyber private key is encrypted with the AES key and stored locally.
   - The Kyber public key is signed by the server and saved with its hash.

2. **Sending a Message**:
   - The sender checks if a shared AES key with the recipient already exists.
     - If it **does**, they decrypt it with their personal AES key and use it to encrypt the message.
     - If it **does not**, they:
       - Use the recipient’s Kyber public key to generate:
         - A shared AES key.
         - A secret, sent to the recipient.
       - The server signs both the message and the secret, then stores them.
       
3. **Receiving a Message**:
   - The recipient requests the message from the server.
   - The server verifies the DSA signature.
     - If **invalid**, the message is discarded.
     - If **valid**, the recipient checks for an existing shared AES key.
       - If it **exists**, it's used to decrypt the message.
       - If not, the recipient requests the secret and derives the shared AES key using their Kyber private key.
       - The new key is stored locally for future use and used to decrypt the message.

---


## 📂 Directory Structure
```bash
SECURE-MSG/
├──SECURE-MSG/
│ ├── cifrados/
│ ├── general users/
│ │   ├── usuario01/
│ │   │   └── firma_clave_publica.txt
│ │   └── usuario02/
│ │       └── firma_clave_publica.txt
│ ├── local users/
│ │   ├── usuario01/
│ │   │   ├── clave_privada.txt
│ │   │   └── usuario02/
│ │   │       └── key.txt
│ │   └── usuario02/
│ │       ├── clave_privada.txt
│ │       └── usuario01/
│ │           └── key.txt
│ ├── static/
│ ├── templates/
│ └── app.py
├── README.md
└── requirements.txt

```
## ▶️ How to Run the Project

### Live Demo

You can check out the app live here:  
🔗 [https://secure-msg.onrender.com](https://secure-msg.onrender.com)

### 1. Clone the repository
```bash
git clone https://github.com/dfedezqui/SECURE-MSG
cd SECURE-MSG
```
### 2. Create and activate a virtual environment
```bash
python -m venv venv
# On Windows
venv\Scripts\activate
# On macOS/Linux
source venv/bin/activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 5. Run the application
```bash
python SECURE-MSG/app.py
```
The server will be available by default at:
👉 http://localhost:5000

## 🛠️ Technologies Used

- **Python** – Core programming language for implementation.
- **Cryptography**:
  - [PyCryptodome](https://www.pycryptodome.org/) – AES encryption and SHA-256 hashing.
  - **DSA** – Digital signature algorithm for authenticity and integrity.
  - **Kyber** – Post-quantum key exchange mechanism.
- **File Management**:
  - Local directory structure:
    - `general/` for public files.
    - `local/<user>/` for user-specific encrypted data.
- **Message Signing & Certificates**:
  - Server signs outgoing data and generates verification certificates for secure communication.

---

## ✍️ Author

Developed by **David Fernández Quiñones** as a personal initiative to explore and demonstrate proficiency in:

- Post-quantum and symmetric cryptography
- Secure client-server architecture
- Cryptographic key management and message signing
- Practical application of hybrid encryption models

This project reflects a hands-on approach to building secure communication systems with a strong emphasis on cryptographic robustness and modular design.


## 📄 License

This project is licensed under the MIT License.  
See the [LICENSE](./LICENSE) file for more details.
