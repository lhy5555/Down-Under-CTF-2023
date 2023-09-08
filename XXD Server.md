# CHALLENGE DESCRIPTION
Description: I wrote a little app that allows you to hex dump files over the internet.

Author: hashkitten

https://web-xxd-server-2680de9c070f.2023.ductf.dev

***Given Files***
[xxd_server (1).zip](./blob/main/xxd_server%20(1).zip)


# Step 1: ANALYSIS

***The Home Page***

![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/e4fbc4f1-1740-4aaf-8b24-5d95d2b85535)

Upon uploading a random file, the `here` redirects to ./uploads/(RANDOM HEX)/(your file name)
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/6e5e104b-3b2d-4cba-9a10-7f8f318691d0)

In the given .htaccess file, we see that other than PHP files, it will be forced as text/plain
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/8875d66e-a64e-4a5f-a03c-f6341aadcb02)


# Step 2: The Exploit
***Crafting the payload***
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/3da5c120-43b3-4227-b8bb-5c25bd127920)
Save this file as shell.php

Then upload it to the homepage and execute it.

# Step 3: Execution (Using Linux)
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/192823b6-2f53-4e77-8570-bbee6b77ee90)

ERROR! Hence we will deploy the DockerFile to find out the issue

# Step 4: Deploy Docker (FOR DEBUGGING THE ERROR)
Command: `docker build -t xxdserver .

![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/50d186db-c320-468b-a49c-6c7605ac1594)

This error arises because there is no flag file in the given files. Hence we will create a fake flag (since we are deploying in our own environment) to solve this issue.


![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/c28c6e2f-53a9-423a-9614-97745990db57)


We will now re-run the docker build command.
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/5fb37f65-deff-4f75-8bac-dcd7d2e36d7a)

No more error:)

Finally we will deploy the docker container with the image
Command: `docker run -it --rm xxdserver`

![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/16c03293-dfee-4d7c-bc95-c8223e018fe8)


Note that it is using the IP Address 172.17.0.2 for this container. Hence we will access it using 172.17.0.2
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/282d1c54-9c3c-41ac-91db-67458da1b91a)

We will upload the shell.php file here.
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/35c272cf-aa02-4656-b9cd-b64a9013c6c0)
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/1ae91175-b0cf-4e44-ab69-4cbb5294e9a7)

We will now debug the file. Opening a new terminal and run the command `docker container ls` to find out your container ID.
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/f7a7ab35-1e19-4631-ba17-78decf282380)

Then we will open a shell in this container
`docker exec -it \[container ID] /bin/bash

![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/996f7cf7-3003-4beb-aab2-834aa6695e78)

Navigate to the location that the shell.php was uploaded at and display the shell.php
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/b9dead8a-b6c7-473a-8e71-10c85ace138a)
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/395fb1c2-a1df-4afa-b2ca-a8a1fc984897)

Based on the debugging of the shell.php, what caused the error is the unexpected **00000010**

# Step 5: Analyse the index.php (DEBUGGING)
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/cb772c8b-9782-4742-b246-22c7db39764f)

Based on this code, it is noticed that it will convert everything in the file to hex dump, and it will split every 16 characters to form a new line. Hence, we will need to modify the shell.php such that it will not read the hex dump, perhaps splitting it into 2 lines where 1 will assign it to variable and the other will execute it.

# Step 6: Modify the shell.php 
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/78f9a005-b692-4089-85f2-8b5d7b03fdd2)

However, do take note that it will split every 16 characters to a new line, hence we need to ensure that it will not read the hex dump contents.
As $_GET['c'] will exceeds 16 characters, we will fill up unnecessary gap with spaces to make up the 16 characters so that the hex dump contents will be commented up.
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/d153af0f-bc7c-4e3f-bc30-6a45348e4066)

# Step 7: Upload the modified shell.php
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/5252f592-0779-42ec-99f3-3244238c485d)

#Step 8: Test
It seems to work now. This error is the to the missing `c` parameter from the URL. To fix that, we will run a test command with the `c` parameter in the URL.
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/89a9679e-099b-4426-a422-b7ea3c7ea58d)

It works! Now lets try to find the location of the flag and display it.
![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/8ffce87c-523a-4012-a813-043826d280a4)


![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/6b8d80ab-0d33-4bbd-878d-4737e4e41889)

It works! So we will now upload this shell.php to the main server.

# Step 8: Upload shell.php to the main server

![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/4f3b9465-1da7-4cf5-8d4a-71c2c5faad67)

![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/cf53b584-8b49-4ba5-a14e-e318d144d4a4)

![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/5cbfec64-8933-4799-b431-7daa3a8572c0)

![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/855cb39f-20cb-41d3-82f7-44c47b624522)

![image](https://github.com/lhy5555/Down-Under-CTF-2023/assets/84282421/ddb622ba-c296-43c1-bdb1-6b313cbf11a3)


Flag: `DUCTF{00000000__7368_656c_6c64_5f77_6974_685f_7878_6421__shelld_with_xxd!}`
