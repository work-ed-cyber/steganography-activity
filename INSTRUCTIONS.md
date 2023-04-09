# Introduction 

Steganography is a commonly used technique for concealing data in a way that it remains undetected. The word "steganography" comes from the Greek words "stegos," meaning "cover," and "grafia," meaning "writing," which refers to the concept of "hidden writing." Steganography is a powerful tool for securing information by embedding it within a cover. It is both an art and science of communication that conceals the presence of communication. While Steganography provides excellent security, it can be further enhanced with Cryptography to achieve better confidentiality and security.

Image Steganography, while having numerous lawful uses, can also be exploited for illicit purposes. For instance, hackers may employ it to distribute viruses and Trojans to infiltrate computer systems, while terrorists and other organizations that depend on clandestine activities can use it for secret and secure communication.

In this steganography lab, you will learn about some of the techniques used to hide information in digital files, how to implement these techniques using various software tools, and how to extract hidden information from digital files.  

# Preparation
- Setup Environments
-- Log into the US Cyber Range
-- Open the Kali Linux Environment	

# Tasks
There are 100s of steganography tools and methods available. We are going to use Outguess (modifies the least significant bits of the digital media file to embed the message), Steghide (uses data compression and encryption to hide messages in digital media files) and Stegseek (a steganographic brute-force utility used to uncover hidden data inside files, https://github.com/RickdeJager/stegseek). 

## Install Outguess
Open a terminal window in your Kali machine and type the following commands:
```sh
sudo apt update
sudo apt install outguess
```
(this updates the package list and installs outguess on your machine). 

### Embedding Data into an Image with Outguess
In this step we will use Outguess to embed data into an image. 

First we need some images to work with. Type the following commands in your Kali terminal to clone the activity files from the repository, change directory into the downloaded repository and list the files. 
```sh
git clone https://github.com/work3d/steganography-activity
cd steganography-activity
ls 
```

Next we want to create a hidden message to embed into one of the image files. To do this type the following into your Kali terminal. Touch creates a file called hidden.txt and the second command opens the newly created file in nano so you can edit it. 

```sh
touch hidden.txt
nano hidden.txt 
```
Enter the message that you want to hide into the nano text editor. 
Once you’ve typed a message, hit ctrl^s to save, followed by ctrl^x to exit. 

You can verify your message by typing: 
```sh
cat hidden.txt 
```
Now embed your message into image1.jpg with the following command:

```sh
outguess -d hidden.txt image1.jpg stego.jpg 
```
Explanation of the command:
- outguess is the name of the OutGuess tool.
- -d hidden.txt specifies the input data file to be hidden in the image. - In this case, the file is hidden.txt.
- image1.jpg is the input image file that will be used to hide the data.
- stego.jpg is the output file, which will be a new image file containing the hidden data.

### Recovering the Hidden Data
In this step, we will use Outguess to recover hidden data from an image. Type the following commands into your Kali terminal:
```sh
outguess -r stego.jpg recovered.txt
cat recovered.txt 
```
Explanation of the command:
- -r tells OutGuess to recover the hidden data from the image.
- stego.jpg is the input stego image file that contains the hidden data.
- recovered.txt is the output file, which will contain the recovered data.

### Embedding Data into an Image that Requires a Password to Recover:

Type the following in your terminal to create a txt file called hidden2.txt and open it in nano: 
```sh
touch hidden2.txt
nano hidden2.txt 
```
Enter the message that you want to hide into the nano text editor. 
Once you’ve typed a message, hit ctrl^s to save, followed by ctrl^x to exit. 

You can verify your message by typing: 
```sh
cat hidden2.txt 
```
Now embed your message into image7.jpg with the following command:
```sh
outguess -k <your-password> -d hidden2.txt -t image7.jpg stego2.jpg
```
Explanation of the command:
- This command will embed the message in hidden2.txt into image7.jpg, using the password <your-password> to encrypt the message. The resulting steganographic image file will be saved as stego2.jpg.
- Note: Replace <your-password> with a password of your choice.
Extracting the Password Protected Message

First try to extract the message using the command that you used previously to recover the message. Does it work? 
Now use: 
```sh
outguess -r -k <your-password> stego2.jpg recovered2.txt
```
Explanation of the command:
- This command will extract the hidden message from the stego2.jpg file using the password <your-password> and save it to a file called retrieved_message.txt in your directory.
- Note: Replace <your-password> with the password you used to encrypt the message.

We have been using Outguess to hide messages in image files. Next we will use Steghide to do the same thing. 

## Install Steghide
Open a terminal window in your Kali machine and type the following command to install steghide (note: run sudo apt update if you did not do it previously)
```sh
sudo apt install steghide
```

### Embedding Data into an Image
First, create a hidden message to hide in the image file called hidden3.txt
```sh
touch hidden3.txt
nano hidden3.txt 
```
Enter the message that you want to hide into the nano text editor. 
Once you’ve typed a message, hit ctrl^s to save, followed by ctrl^x to exit. 

As before, you can verify your message by typing: 
```sh
cat hidden3.txt 
```
Now embed your message into image3.jpg with the following command:
```sh
steghide embed -cf image3.jpg -ef hidden3.txt
```
Enter a passphrase when prompted
Explanation of the command:
- steghide: This is the command to run the steghide program.
embed: This tells steghide to embed a file into an image file.
- -cf image3.jpg: This is the cover file, which is the image file that the hidden data will be embedded into. In this case, it is image3.jpg.
- -ef hidden3.txt: This is the file that will be embedded into the cover file. In this case, it is hidden3.txt.

### Extracting the Password Protected Message
Type the following command in your terminal:
```sh
steghide extract -sf image3.jpg
```
Enter passphrase when prompted. 
The output of the command will display the extracted message

Explanation of the command: 
- This command extracts a hidden message from a JPEG image named image3.jpg using steghide. The -sf  option specifies the file from which to extract the message. In this case, it's image3.jpg.
- Steghide will search for any hidden message within the image, and if found, it will extract and display it on the command line. If the image does not contain any hidden message, the command will display an error message.

## Using stegseek to brute-force passwords of Steganography Files
Stegseek is a tool used to uncover hidden messages within a file that has been used for steganography. It does this by trying every possible key until the message is discovered or all possibilities are exhausted. This tool can be used to extract hidden messages from many types of files.

We will be manually installing stegseek. The process is to first install the dependencies for stegseek, download the stegseek source code from github and compile it. 

You will need to following dependencies:
- libmhash-dev
- libmcrypt-dev
- libjpeg-dev
-- Can either the independent JPEG version or the libjpeg-turbo version
-- The libjpeg version must be below 9
- lib1g-dev

To install these, open your terminal in Kali linux and type (note: if you ran sudo apt update in the previous section, you can skip this command): 
```sh
sudo apt update
sudo apt install libmhash-dev libmcrypt-dev libjpeg-dev zlib1g-dev
```
You will also need the following tools to compile /build the source code once it is downloaded: 
- make 
- g++
- cmake
- git

To install these, type the following command:
```sh
sudo apt install git build-essential cmake
```
To get the stegseek source code, use the following command:
```sh
git clone https://github.com/RickdeJager/stegseek.git
```
Last, you need to compile and install stegseek. Enter the following commands into your Kali terminal: 
```sh
cd stegseek
mkdir -p build
cd build
cmake -DCMAKE_BUILD_TYPE=Release .. 
make
sudo make install
```
Verify that stegseek is installed by typing: 
```sh
stegseek
```
You should see the stegseek menu of options if it was successfully installed. 

The basic command to brute-force a stego file is: 
```sh
stegseek <stego_file.jpg> <wordlist.txt>
```
This will try every password in the provided wordlist against your stego_file.jpg. 

Kali Linux comes with several pre-installed wordlists that can be used for password cracking. You can find them in the /usr/share/wordlists/ directory.
Here are some of the commonly used wordlists available in Kali Linux:
rockyou.txt: This is a popular wordlist that contains over 14 million passwords that have been leaked in various data breaches. 
seclists: This is a collection of multiple wordlists that includes common passwords, usernames, and other useful lists for password cracking.
john: This is a directory that contains wordlists used by the John the Ripper password cracking tool.

You can browse the /usr/share/wordlists/ directory to see the full list of available wordlists with the following command:
```sh
ls /usr/share/wordlists/ 
````
You may need to unzip the rockyou.txt wordlist in order to use it. If you see rockyou.txt.gz, type the following command to unzip it: 
```sh
sudo gunzip /usr/share/wordlists/rockyou.txt.gz
```

### Cracking Stegonography Files with Stegseek 
Use stegseek to brute force the steganography file that you created with steghide previously. 

First, erify that you are in the same directory that contains your image files for the steganography lab. Once you are in the correct directory, type the following command:
```sh
stegseek image3.jpg /usr/share/wordlists/rockyou.txt
```
Did it crack your password? How long did it take? 

Three of the images in the directory contain hidden messages. See if you can find them. You’ll only be able to find two of the hidden messages with stegseek. To find the third message research various methods used in steganography. You don't need to install any further tools. The tool you need to find the third message is a commonly used tool. 

Hint: dW56aXA=
