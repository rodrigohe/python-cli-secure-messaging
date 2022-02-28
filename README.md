# Python CLI Secure Messaging

## About The Project
**Python CLI Secure Messaging** is a fast, secure command-line based chat application built entirely in Python. Chats are performed in real-time between two clients who establish a shared connection. This project follows a client-server model, where the running server acts as an intermediary to service all clients.

https://user-images.githubusercontent.com/64722294/155913647-cc58bebe-768a-4d5d-bdae-29912a1be67c.mp4

Every client and server runs in a multi-threaded environment leveraging Python's *threading* module. Once logged into the system, a running client spawns a sender thread and a receiver thread, to handle sending requests and receiving responses respectively. To handle multiple clients, the server spawns a new thread dedicated to each client connection established over a TCP socket. Concurrency allows each client and the main server to perform multiple tasks at once, and achieve proper synchronization between tasks.

The following features are included with this application:
- 1:1 real-time chat messaging
- text messages
- image messages
- reliable message histories (while a user session stays open)
- images retrieved from histories are organized in the client's filesystem
- account registration & deletion

Python CLI Secure Messaging achieves the following security features:
- message integrity is assured
- users authenticate with each other before chatting
- perfect forward secrecy is maintained
- end-to-end encryption is maintained
- at-rest encryption is maintained
- users can deny having sent a message (deniability)
- password inputs for the application are hidden from view for privacy

### Message Encryption Overview
To achieve end-to-end encryption with perfect forward secrecy we've implemented a derivation of Signal Protocol [[1]](https://en.wikipedia.org/wiki/Signal_Protocol). This application uses the Extended Triple Diffie-Hellman (X3DH) to derive a shared key between two users and then encrypts messages using an AEAD encryption scheme (AES-GCM) with the derived shared key. We chose X3DH due to its deniability property, X3DH doesn't give either Alice or Bob a publishable cryptographic proof of the contents of their communication or the fact that they communicated [[2]](https://signal.org/docs/specifications/x3dh/).

## Getting Started
### Dependencies
Python version 3.8+ was used for development of this application, and is suggested to be used for correct program behaviour.

Multiple Python libraries were used to achieve the functionality of this program. The following dependencies must be installed via *pip*:
- Python Standard Library
- cryptography
- Pillow (PIL)
- termcolor

### Running `sm_server.py`
The server runs on Port 50007, and must be started first to initiate the program.
1. From the root of the repository, run the following command in a terminal:
`python python/server/sm_server.py`

### Running `sm_client.py`
Clients run on their own distinct Port numbers, and each in their own terminal window. They can be started once a server is running.

2. From the root of the repository, run the following command in a different terminal:
`python python/client/sm_client.py`

3. Repeat Step 2 in new terminals to start more new clients.

4. From here, application menus provide detailed descriptions of features accessible to each client user. In most menus, users have the option to display all present options that they have from a specific menu. This can be shown with the following command at the user's in-application command prompt:
`--options`
### Quitting the program
Clients can individually logout and quit the program via in-application commands.
The server runs indefinitely and must be killed externally from another program, to free up Port 50007 from the running server process. An example of a command to achieve this (in Windows) is:
`npx kill-port 50007`

###
