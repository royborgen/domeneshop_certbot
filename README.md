# Domeneshop Certbot DNS Authentication Script

This Python script automates the process of setting up DNS authentication for SSL certificate renewal using Certbot. It integrates with the Domeneshop API to manage DNS records for the specified domain.

## Features

- Automatically creates or updates DNS TXT records required for Certbot DNS authentication.
- Integrates with Domeneshop API for DNS management.

## Prerequisites

Before using this script, ensure you have the following:

- Python 3 installed on your system.
- Certbot installed and configured for DNS authentication.
- Access to the Domeneshop API with necessary permissions.

## Installation

1. Clone or download the repository to your local machine.

2. Ensure you have the required Python dependencies installed:
    - Python 3 
    - requests library installed 
    - sys library installed 
    - json library installed
    - os library installed
    - configparser library installed
    - re library installed

## Set up API credentials for Domeneshop:
1. Obtain an API token and secret from Domeneshop: https://domene.shop/admin?view=api
2. For security is is recommended to set API credentials as enviroment variables **DOMENESHOP_API_TOKEN** and **DOMENESHOP_API_SECRET**. 

## Configuration
Modify the script variables according to your domain setup:
- **ROOT_DOMAIN:** The root domain for which the SSL certificate is being renewed.
- **SUBDOMAIN:** The subdomain used for Certbot DNS authentication.
- **TTL:** Time-to-live for DNS records (in seconds).

## Usage
Make sure the script (domeneshop_certbot.py) has execute permissions:
 ```
chmod +x domeneshop_certbot.py
 ```
Run the script integrate with Certbot:
 ```
sudo certbot certonly --manual --preferred-challenges dns --manual-auth-hook /path/to/domeneshop_certbot.py -d subdomain.domain
 ```
