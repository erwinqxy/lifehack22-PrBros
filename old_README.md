<p align="center">
    <h1 align="center"> Agent Maverick </h1>
</p>

<p align="center">
    <img src="images/photo_2022-07-10_05-40-26.jpg" />
</p>

<p align="center">
    <h3 align="center"> Telegram handle: @stegEncryptBot [https://t.me/stegEncryptBot] </h3>  
    <h4 align="center"> lifehack22-PrBros (LifeHack 2022) </h4>
</p>

## Set up Python Virtual Environment 
- `python3 -m pip install --user virtualenv`
- `python3 -m venv env`
- `source env/bin/activate`
- To leave: `deactivate`
- `pip3 install -r requirements.txt` 

## LifeHack 2022 Submission 
- [Devpost](https://devpost.com/software/steganography-encryption-bot?ref_content=user-portfolio&ref_feature=in_progress)
- [Video Demo](https://www.youtube.com/watch?v=9460xClGWlA&t=1s)

## How to bring Agent Maverick alive
First, get a Telegram bot API key from @BotFather. 
Once the repo is cloned, create a .env file and insert the line below:

```API_TOKEN = 'INSERT API TOKEN HERE'```

Navigate to `telegramBot/telegramBot.py` and run the file. 

Agent Maverick becomes live the moment the file is run.
We will be hosting the bot from 1200 to 1800 hours. Do give the bot time between messages. 
If you are facing issues hosting the bot or using it during the period, feel free to contact any of us via email and we will look into it or arrange a time to host the bot for you to test it out.

## Inspiration
Ever had the need to encrypt a message, but not want it to look painfully obvious that it has been encrypted? We do! From personal details to bank account numbers, there are plenty of instances where encryption comes in handy. It doesn't just end here in our project, Agent Maverick. In Agent Maverick, we take it a notch higher: encrypted messages are further embedded into images provided by end-users.

We decided to create a Telegram bot as our end-user service interface because Telegram is a cross-platform messaging platform that is becoming increasingly popular. A Telegram bot would also suffice as a working proof of concept.

## What it does
**Agent Maverick** is a Telegram bot that takes in a PNG (only) image alongside the secret message (ASCII characters) that the user intends to hide. We encrypt the secret message using Caesar cipher, then embed the ciphertext into the image provided via image steganography. The resultant image is then sent back to the user. 

There are 2 key functionalities in our project:

### Cryptography (Caesar Cipher)
It is a type of substitution cipher in which each letter in the plaintext is 'shifted' a certain number of places down the alphabet. For example, with a shift of 1, A would be replaced by B, B would become C, and so on. It is a type of substitution cipher in which each letter in the plaintext is 'shifted' a certain number of places down the alphabet. For example, with a shift of 1, A would be replaced by B, B would become C, and so on.
When encrypting, a person looks up each letter of the message in the "plain" line and writes down the corresponding letter in the "cipher" line.

Plaintext:  THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG

Ciphertext: QEB NRFZH YOLTK CLU GRJMP LSBO QEB IXWV ALD

Deciphering is done in reverse, with a right shift of 3.

![Image steganography](images/1200px-Caesar_cipher_left_shift_of_3.svg.png "Caesar Cipher")

### Image Steganography (Least Significant Bit (LSB) Approach)
Digital images may be described as finite sets of pixels. Pixels are in turn defined to be the smallest individual element of an image. They hold values to represent the brightness of a given colour at any specific point. As such, we can think of images as a matrix of pixels.

In the LSB approach, we replace the last bit of each pixel with each bit of our ciphertext. Each pixel contains 3 values: Red, Green and Blue. These values range from 0 to 255. By encrypting and converting the secret message into binary, we iterate over the pixel values 1 by 1, replacing each LSB with the ciphertext bits sequentially. Since we are only modifying pixel values by +1 or -1, any changes in the resultant image will be indistinguishable to the human eye.

<p align="center">
    <img src="images/image_steganography.png" />
</p>

## How we built it

### Telegram Bot
We utilised the `pyTelegramBotAPI` library to build **Agent Maverick**. Images that are sent to Agent Maverick are stored locally on the host's computer for either encryption or decryption to be done. The images are deleted from the host's computer when the user invokes the `Delete` function. `chatIDs` and `messageIDs` are stored to facilitate the implementation of a Delete function which clears all sensitive text. The IDs are deleted subsequently. Due to a limitation on Telegram's part, messages can only be deleted if they were sent less than 48 hours ago. 

We used custom keyboards instead of verbose commands to streamline the user experience.

We are also proud to announce that Agent Maverick is capable of supporting multiple users at the same time.

### Cryptography
Due to the simplicity of the cipher, we were able to write the encryption and decryption functions of the Caesar cipher from scratch. The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the scheme, A → 0, B → 1, ..., Z → 25. Encryption of a letter x by a shift n can be described mathematically as,

$${\displaystyle E_{n}(x)=(x+n)\mod {26}.}$$

Decryption is performed similarly,

$${\displaystyle D_{n}(x)=(x-n)\mod {26}.}$$

### Image Steganography
Image steganographic functionalities are built with `NumPy` and `opencv-python`. We settled for NumPy because we can enjoy the flexibility of `Python` and the speed of compiled C code at its core. What is more, is that NumPy indexing is the de facto standard of array computing today. `OpenCV` was another obvious choice for us as it is one of the famously used open-source `Python` libraries meant exclusively for Computer Vision. Modules and methods available in OpenCV allow us to perform image processing with a few lines of code. We wanted to make use of this hackathon to learn a little more about Computer Vision techniques.

## Challenges we ran into
We experimented with discrete cosine transform as our implementation for image steganography. Unfortunately, the resultant image has a tinge of a strange blue hue to it. As a result, we scrapped our work and started from square 1 again. This time, we looked to modify the least significant bit of each pixel in the image. We are glad that the resultant image turned out to be indistinguishable from the original, at least to the human eye.

As it was our first time working with complex cryptographic methodologies such as the AES and the RSA, it was unfortunate that we were unable to overcome this obstacle head-on. Instead, we found a workaround to the issue. By turning to the simpler Caesar cipher, we were able to come up with a working product to serve as a working proof of concept. We believe this is the most rational way forward.

## Accomplishments that we're proud of
We are immensely proud of the fact that we were able to come up with a working product in under 24 hours that has viable use cases in the real world. In hindsight, we felt that we handled obstacles along the way really well despite treading into unknown waters. Regardless of the outcome of the competition, we are glad that we will be walking away with newfound knowledge in the field of computer science.

## What we learned
From discrete cosine transforms to replacing the least significant bits in the pixels, we learnt a ton about image steganography and the mathematical intuition behind it. We thought it was a great introduction to the subject of media computing. We are also glad that we decided on the security theme, for it enabled us to learn more about modern cryptographic concepts; their performances, their effectiveness, as well as their tradeoffs.

## What's next for Agent Maverick
We hope that we will be able to include an array of encryption methods for the users to choose from to encrypt their secret messages before embedding them into images. A few notable encryption schemes that we hope to integrate into our Telegram bot are AES, RSA, Vignere cipher as well as the keyword cipher. On top of that, we are looking to allow users to opt for audio steganography as well. For that, we plan to rely on Fourier Transforms to achieve the desired outcome.

Additionally, we hope to deploy it on Heroku such that Agent Maverick is available for everyone who needs it. Apart from that, we are looking to store text messages and images in the cloud. We believe that Amazon S3 would be perfect for this job.
