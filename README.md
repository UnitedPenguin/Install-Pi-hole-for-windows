# Pi-hole Installation on Windows

Pi-hole offers Network-wide Ad Blocking, allowing not just standard ad-blocking, but also connecting to your router so every device on your network benefits from ad-blocking. This includes devices that aren't traditionally easy to set ad-blockers on like mobile devices, smart TVs, etc.

## Prerequisites

- Docker: [Download Docker](https://www.docker.com/products/docker-desktop/)

## Installation Steps

1. **Install Docker:** After downloading, install Docker on your system.
2. **Open Docker:** Once Docker is up and running, select the WSL 2 backend. This should be the default option.
3. **Download Pi-hole:** Open a command prompt as an administrator (or use PowerShell if command prompt doesn't work) and run the following command:
    ```
    docker pull pihole/pihole
    ```
4. **Assign a Static IP to Your PC:** 

   a. Open system settings, then navigate to "Network and Internet". 
   
   b. Note the network under status; this is likely the name of your Wi-Fi or Ethernet connection.
   
   c. Proceed to "Network and Sharing Center" > "Change Adapter Settings".
   
   d. Right-click on your active network and select "Properties", then choose "Internet Protocol Version 4 (TCP/IPv4)".
   
   e. Open a command prompt and run `ipconfig` to view your current network details.
   
5. **Setup Static IP:** In the "Internet Protocol Version 4 (TCP/IPv4)" settings, choose "Use the following IP address" and input the details from `ipconfig`. Set the DNS server to the IP address you noted, and for the alternative DNS, use `1.1.1.1`.

6. **Run Pi-hole using Docker:** 
    ```
    docker run -d --name pihole -e ServerIP=YOUR_IP_ADDRESS -e WEBPASSWORD=YOUR_CHOSEN_PASSWORD -e TZ=YOUR_TIMEZONE -e DNS1=127.17.0.1 -e DNS2=1.1.1.1 -e DNS3=1.0.0.1 -p 80:80 -p 53:53/tcp -p 53:53/udp -p 443:443 --restart=unless-stopped pihole/pihole:latest
    ```
    Make sure to replace:
    - `YOUR_IP_ADDRESS` with your actual IP address.
    - `YOUR_CHOSEN_PASSWORD` with a password of your choosing.
    - `YOUR_TIMEZONE` with your respective timezone (optional but recommended).

    Note: For DNS settings, `DNS1` must remain as `127.17.0.1`.

7. **Verify Installation:** Go to [http://127.17.0.1/admin/](http://127.17.0.1/admin/) or [http://localhost/admin/](http://localhost/admin/). If prompted for a password, enter the one you set during the docker run command.

8. **Router Configuration:** To extend Pi-hole benefits to your entire network, set the DNS server on your router to the IP address you noted from the `ipconfig` command.

## Post-Installation Notes

- If you restart your computer and face internet issues:
    1. Repeat step 5, setting the DNS to `1.1.1.1`.
    2. Open a command prompt as admin and run:
        ```
        ipconfig /flushdns
        ipconfig /registerdns
        ```
    3. Ensure Docker is running. Once it's up, revert to the original IP and DNS settings you set initially. If issues persist, try restarting your PC.

- Troubles with Docker commands? Instead of copying and pasting, try typing them out. Sometimes, PowerShell may have issues with formatting.

- If you encounter an error stating "docker desktop WSL kernel version too low":
    ```
    wsl --update
    ```
    And if the issue persists:
    ```
    wsl --install
    wsl --update
    ```

Happy ad-free browsing!
