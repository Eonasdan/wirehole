
## What is this?

WireHole is a docker-compose project that combines WireGuard, PiHole, and Unbound to create a full or split-tunnel VPN that is easy to deploy and manage. This setup allows for a VPN with ad-blocking via PiHole and enhanced DNS privacy and caching through Unbound.

## Author

👤 **Jonathan Peterson**

- Twitter: [@Eonasdan](https://twitter.com/Eonasdan)
- GitHub: [@Eonasdan](https://github.com/Eonasdan)

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/Eonasdan/wirehole/issues).

## Show your support

Give a ⭐ if this project helped you!

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/eonasdan)

---

### Quickstart

To begin using WireHole, clone the repository and start the containers.

```bash
# Clone the WireHole repository from GitHub
git clone https://github.com/eonasdan/wirehole.git

# Copy the data folder to the root directory
cp wirehole/data /data

# Enter the docker folder
cd /data/docker

# Update the .env file with your configuration
cp .env.example .env
nano .env

# Start the Docker containers
docker compose up -d
```

Remember to set secure passwords for `PASSWORD`, and `WEBPASSWORD` in your `.env` file.

---

## Environment Configuration Details

The `.env` file contains a series of environment variables that are essential for configuring the WireHole services within the Docker containers. Here is a detailed explanation of each variable:

### General Settings

- `TIMEZONE`: Sets the timezone for all services. It is important for logging and time-based operations. Example: `America/Los_Angeles`.

### User / Group Identifiers

- `PUID` and `PGID`: These ensure that the services have the correct permissions to access and modify files on the host system. They should match the user ID and group ID of the host user.

### Network Settings

- `UNBOUND_IPV4_ADDRESS`: The static IP address assigned to Unbound, ensuring it is reachable by Pi-hole.
- `PIHOLE_IPV4_ADDRESS`: The static IP address assigned to Pi-hole, allowing it to serve DNS requests for the network.
- `WG_IPV4_ADDRESS`: The static IP address for WireGuard Easy.

### WireGuard Settings

- `WG_HOST`: Your WAN IP, or a Dynamic DNS hostname.
- `WG_PORT`: The public UDP port of your VPN server. WireGuard will always listen on 51820 inside the Docker container.
- `WG_DEFAULT_ADDRESS`: Clients IP address range.
- `WG_DEFAULT_DNS`:  DNS server clients will use. If set to blank value, clients will not use any DNS.
- `PASSWORD`: A password to log in on the Web UI.

### Pi-hole Settings

- `WEBPASSWORD`: The password for accessing the Pi-hole web interface. It should be set to a secure value to prevent unauthorized access.
- `PIHOLE_DNS`: The IP address of the Unbound server used by Pi-hole to resolve DNS queries.
- There are other settings that you can see an example in the `env.example` file. Check the Pi-hole docker repo for more.

Remember to replace any default or placeholder values with secure, unique values before deploying your services.

---

### Recommended Configuration / Split Tunnel

For a split-tunnel VPN, configure your WireGuard client `AllowedIps` to `10.2.0.0/24`, which will route only the web panel and DNS traffic through the VPN.

---

### Accessing the Web Panel (WireGuard Easy)

Manage your WireGuard VPN through the WireGuard-UI at:

`http://{YOUR_SERVER_IP}:51821`

Log in with the `PASSWORD` you have set in your `.env` file.

### Access PiHole

Connect to WireGuard and access the Pi-hole admin panel at `http://{YOUR_SERVER_IP}/admin`. The login password is the one set as `WEBPASSWORD` in your `.env` file.

---

### Dynamic DNS (DDNS)

Configure DDNS by setting `WG_HOST` in your `.env` file to your DDNS URL.

---

### Support and Updates

To update Unbound and WG-Easy to the latest image: simply stop, and remove the containers and run `docker compose up -d` again. You may want to run `docker pull [image]` before otherwise, you may have an issue resolving the image download.

The Pi-hole docker team recommends that you target a specific version of the image and not use the `latest`. You can modify the docker-compose file to update the tag.

---

###### Acknowledgements

* [@klutchell](https://github.com/klutchell/unbound-docker) for his unbound docker image.
* [Pi-hole](https://github.com/pi-hole/docker-pi-hole) for their docker image and the Pi-hole project.
* [wg-easy](https://github.com/wg-easy/wg-easy) for their docker image.
* [@DevinStokes](https://github.com/IAmStoxe) for his original wirehole repo.
