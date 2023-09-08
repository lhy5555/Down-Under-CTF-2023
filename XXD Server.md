# CHALLENGE DESCRIPTION
Description: I wrote a little app that allows you to hex dump files over the internet.

Author: hashkitten

https://web-xxd-server-2680de9c070f.2023.ductf.dev

***Given Files***
[xxd_server (1).zip](./blob/main/xxd_server%20(1).zip)


# Step 1: ANALYSIS

***The Home Page***

![image](./image/image%201.png)

Upon uploading a random file, the `here` redirects to ./uploads/(RANDOM HEX)/(your file name)
![image](./image/image%202.png)

In the given .htaccess file, we see that other than PHP files, it will be forced as text/plain
![image](./image/image%203.png)


# Step 2: The Exploit
***Crafting the payload***


![image](./image/image%204.png) 



Save this file as shell.php

Then upload it to the homepage and execute it.

# Step 3: Execution (Using Linux)
![image](./image/image%205.png)

ERROR! Hence we will deploy the DockerFile to find out the issue

# Step 4: Deploy Docker (FOR DEBUGGING THE ERROR)
Command: `docker build -t xxdserver .

![image](./image/image%206.png)

This error arises because there is no flag file in the given files. Hence we will create a fake flag (since we are deploying in our own environment) to solve this issue.


![image](./image/image%207.png)


We will now re-run the docker build command.
![image](./image/image%20E8.png)

No more error:)

Finally we will deploy the docker container with the image
Command: `docker run -it --rm xxdserver`

![image](./image/image%209.png)


Note that it is using the IP Address 172.17.0.2 for this container. Hence we will access it using 172.17.0.2


We will upload the shell.php file here.
![image](./image/image%2010.png)
![image](./image/image%2011.png)

We will now debug the file. Opening a new terminal and run the command `docker container ls` to find out your container ID.
![image](./image/image%2012.png)

Then we will open a shell in this container
`docker exec -it \[container ID] /bin/bash

![image](./image/image%2013.png)

Navigate to the location that the shell.php was uploaded at and display the shell.php
![image](./image/image%2014.png)
![image](./image/image%2015.png)

Based on the debugging of the shell.php, what caused the error is the unexpected **00000010**

# Step 5: Analyse the index.php (DEBUGGING)
![image](./image/image%2016.png)

Based on this code, it is noticed that it will convert everything in the file to hex dump, and it will split every 16 characters to form a new line. Hence, we will need to modify the shell.php such that it will not read the hex dump, perhaps splitting it into 2 lines where 1 will assign it to variable and the other will execute it.

# Step 6: Modify the shell.php 
![image](./image/image%2017.png)

However, do take note that it will split every 16 characters to a new line, hence we need to ensure that it will not read the hex dump contents.
As $_GET\['c'] will exceeds 16 characters, we will fill up unnecessary gap with spaces to make up the 16 characters so that the hex dump contents will be commented up.
![image](./image/image%2018.png)

# Step 7: Upload the modified shell.php
![image](./image/image%2019.png)

# Step 8: Test
It seems to work now. This error is the to the missing `c` parameter from the URL. To fix that, we will run a test command with the `c` parameter in the URL.
![image](./image/image%2020.png)

It works! Now lets try to find the location of the flag and display it.
![image](./image/image%2021.png)


![image](./image/image%2022.png)

It works! So we will now upload this shell.php to the main server.

# Step 9: Upload shell.php to the main server

![image](./image/image%2023.png)

![image](./image/image%2024.png)

![image](./image/image%2025.png)

![image](./image/image%2026.png)

![image](./image/image%2027.png)


Flag: `DUCTF{00000000__7368_656c_6c64_5f77_6974_685f_7878_6421__shelld_with_xxd!}`
