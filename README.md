# Project-Pi-hole
Secure Home Lab &amp; Ad-Blocking Server: Deployed a network-wide ad-blocker on a Raspberry Pi using Docker, and made it securely accessible via a Cloudflare Tunnel to bypass ISP limitations.
1. Objective
The goal of this project was to build a foundational home lab to gain practical, hands-on experience in system administration, containerization, and network security. The primary outcome was to deploy a network-wide DNS ad-blocker (Pi-hole) and make its administrative interface securely accessible from the public internet, bypassing common ISP limitations. This project serves as a practical application of concepts learned in my BSIT program and demonstrates core skills relevant to modern IT operations.
2. Technologies Used
Hardware:
Raspberry Pi 5 (8GB Model)
1TB NVMe SSD with a PCIe HAT
Operating System:
Raspberry Pi OS Lite (64-bit)
Core Software & Services:
Docker: For containerizing applications and isolating services.
Portainer: A web-based GUI for managing Docker containers, volumes, and networks.
Pi-hole: A DNS sinkhole that acts as a network-wide ad-blocker.
Cloudflare Tunnels: A secure tunneling service to expose internal services to the internet without port forwarding.
Networking & Management:
SSH: For secure command-line access to the server.
Custom Domain: erdlesdomain.org for public access.
Cloudflare DNS: For managing the public DNS records of the custom domain.
Eero Mesh System: For managing the local network and DHCP settings.
3. Process & Implementation
Phase 1: Server Provisioning and OS Setup
Hardware Assembly: Assembled the Raspberry Pi 5 with its NVMe HAT and 1TB SSD.
OS Installation: Flashed Raspberry Pi OS Lite (64-bit) to the NVMe drive. This was done directly to maximize performance and avoid using a slower SD card as an intermediary.
Headless Configuration: During the flashing process using Raspberry Pi Imager, I pre-configured the OS for headless operation by setting the hostname, creating a user account, and enabling SSH.
NVMe Boot: Edited the Pi's boot configuration file (config.txt) to enable the PCIe bus, allowing the system to boot directly from the high-speed NVMe drive.
Phase 2: Containerization Platform
Docker Installation: Installed Docker Engine using the official convenience script. This provides the core platform for running containerized applications.
Portainer Deployment: Deployed Portainer as a Docker container to provide a web-based management interface for the Docker environment, simplifying future deployments.
Phase 3: Pi-hole Deployment
Stack Definition: Deployed Pi-hole using a Docker Compose stack within Portainer. This method ensures the deployment is reproducible and easy to manage.
Configuration: Configured the Pi-hole service with a static IP and a secure web admin password.
Network Integration: Configured the Eero mesh system's DHCP settings to use the Pi-hole server as the primary DNS for all devices on the network, enabling network-wide ad blocking.
Phase 4: Secure Remote Access
Challenge Identification: My ISP uses Carrier-Grade NAT (CGNAT), which prevents traditional port forwarding and makes it impossible to host a public service directly.
Solution - Cloudflare Tunnel:
Registered a custom domain (erdlesdomain.org) and configured it with a free Cloudflare account.
Created a Cloudflare Tunnel and installed its connector (cloudflared) as a Docker container on the Raspberry Pi.
Established a dedicated Docker network (homelab-net) to allow the cloudflared container and the pihole container to communicate directly and reliably by name.
Configured a public hostname (pihole.erdlesdomain.org) in the Cloudflare dashboard to route traffic through the secure tunnel directly to the Pi-hole container's web interface.
4. Challenges & Solutions
Challenge: The Raspberry Pi 5 did not initially recognize the NVMe drive.
Solution: I diagnosed the issue as a disabled PCIe bus. By adding dtparam=pciex1_gen=3 to the system's config.txt file, I successfully enabled the hardware, allowing the OS to boot from the SSD.

Challenge: The Cloudflare Tunnel initially had trouble connecting to the Pi-hole container (502 Bad Gateway error).
Solution: I resolved this by creating a dedicated Docker bridge network. Connecting both containers to this shared network allowed them to resolve each other by name (http://pihole:80), creating a stable and robust internal communication path that is independent of the host's IP address.
5. Outcome & Key Learnings
This project successfully established a low-power, high-performance home server that provides a valuable service to my home network. The Pi-hole dashboard is now securely accessible from anywhere in the world via a custom domain, despite ISP network restrictions.
Key skills demonstrated:
Linux System Administration: Headless server setup, command-line navigation, package management, and system configuration.
Containerization: Proficient use of Docker to deploy, manage, and isolate applications.
Network Troubleshooting: Diagnosed and solved complex connectivity issues related to CGNAT and internal Docker networking.
DNS & Domain Management: Configured a custom domain and leveraged Cloudflare for DNS management and security services.
Security: Implemented a secure method for remote access that is superior to traditional port forwarding.





