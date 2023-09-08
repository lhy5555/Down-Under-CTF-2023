# CHALLENGE DESCRIPTION
Description: I wrote a little app that allows you to hex dump files over the internet.

Author: hashkitten

https://web-xxd-server-2680de9c070f.2023.ductf.dev

***Given Files***
[xxd_server (1).zip](https://github.com/lhy5555/Down-Under-CTF-2023/files/12560889/xxd_server.1.zip)


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

