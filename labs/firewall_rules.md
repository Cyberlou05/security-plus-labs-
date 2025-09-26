# Firewall rules Demo (Securiity+ Labs) 

**Purpose:** Demonstrate basic host firewall rules (iptables ? UFW) and AWS Security Group guidance.

## Local Linux (iptables) examples - *testin a VM only*
#Allow loopback
sudo iptables -A INPUT -i lo -j ACCEPT

# Allow established connections 
sudo iptables -A INPUT -m contrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Allow ssh (restrict to your IP on produciton) 
sudo iptables -A INPUT -pp tcp --dport 22 -m conntrack --cststate new -j ACCEPT 

#Allow HTTP/HTTPS
sudo iptables -A INPUT -p tcp -- dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT 

#Default drop (last resort) 
sudo iptables -p INPUT DROP 

**Notes** 
- Do not run production systems without console access.
- For SSH in production, resctrict source CIDR to your IP (e.g., 203.0.113.45/32)

## Ubuntu (ufw) quick example 
sudo ufw reset 
sudo ufw deafult deny incoming 
sudo ufw default allow outgoing 
sudo ufw allow from 203.0.113.45 to any port 22 comment 'SSH from my IP' 
sudo ufw allow 80/tcp 
sudo ufw allow 443/tcp
sudo ufw enable 
sudo ufw status verbose 

## AWS Security Group guidance (console / CLI)
- SSH (22) → Source: your IP (NOT 0.0.0.0/0)
- HTTP (80) → Source: 0.0.0.0/0
- HTTPS (443) → Source: 0.0.0.0/0
- For management ports, prefer a bastion host or VPN.

**Why this matters (Security+ tie-in)**
- Minimizing exposed ports reduces attack surface.
- “Allow only what you need” is a core least-privilege practice.
