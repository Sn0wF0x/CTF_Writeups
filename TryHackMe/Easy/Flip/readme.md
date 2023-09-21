# Writeup for: TryHackMe 0x41haz

Difficulty: Easy

Cathegory: Crypto/Cipher/AES

File Attached: App.py


## Infos:

The Server is listening on Port 1337

### Step 1:

Lets first open "app.py" and take a look at it

We have 5 Functions

-> encrypt_data(data,key,iv)
-> send_message(server,message)
-> setup(server,username,password,key,iv)
-> start(server)
-> decrypt_data(encryptedParams,key,iv)

This looks interesting: 

if b'admin&password=sUp3rPaSs1' 




