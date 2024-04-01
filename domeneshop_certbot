#!/usr/bin/python3
import time
import os
import requests
import json

# API endpoint and credentials
API_URL = "https://api.domeneshop.no/v0/domains"
API_TOKEN = os.environ.get("DOMENESHOP_API_TOKEN")
API_SECRET = os.environ.get("DOMENESHOP_API_SECRET")

# Domain information
ROOT_DOMAIN = "EXAMPLE.COM"
SUBDOMAIN = "_acme-challenge.SUBDOMAIN"

# Validation token provided by Certbot
TOKEN_VALUE = os.environ.get("CERTBOT_VALIDATION")

# Default TTL for DNS records
TTL = 300

def get_domain_id():
    response = requests.get(API_URL, auth=(API_TOKEN, API_SECRET))
    domains = response.json()
    for domain in domains:
        if domain["domain"] == ROOT_DOMAIN:
            return domain["id"]
    return None

def create_dns_record(domain_id):
    # Check if the DNS record already exists
    response = requests.get(f"{API_URL}/{domain_id}/dns", auth=(API_TOKEN, API_SECRET))
    existing_records = response.json()
    for record in existing_records:
        if record["host"] == SUBDOMAIN and record["type"] == "TXT":
            print("Found existing record")
            record_id = record["id"]
            print("Updating record: ", record_id)

            # Update the existing DNS record
            payload = {
                "host": SUBDOMAIN,
                "ttl": TTL,
                "type": "TXT",
                "data": TOKEN_VALUE
            }
            headers = {
                "Content-Type": "application/json"
            }
            response = requests.put(f"{API_URL}/{domain_id}/dns/{record_id}", json=payload, auth=(API_TOKEN, API_SECRET), headers=headers)
            return response.status_code, response.text

    # If the DNS record does not exist, create a new one
    print("Creating new record")  # Add this logging statement
    payload = {
        "host": SUBDOMAIN,
        "ttl": TTL,
        "type": "TXT",
        "data": TOKEN_VALUE
    }
    headers = {
        "Content-Type": "application/json"
    }
    response = requests.post(f"{API_URL}/{domain_id}/dns", json=payload, auth=(API_TOKEN, API_SECRET), headers=headers)
    return response.status_code, response.text

def main():
    domain_id = get_domain_id()
    if domain_id is None:
        print(f"Domain ID not found for {ROOT_DOMAIN}")
        return
    else:
        print(f"Found domain ID: {domain_id}")

    status_code, response_text = create_dns_record(domain_id)
    if status_code == 201 or status_code == 204:
        print(f"TXT record created/updated successfully for {SUBDOMAIN}.{ROOT_DOMAIN}")
        print("Waiting 30 seconds before proceeding with Certbot verification")
        time.sleep(30)
    else:
        print(f"Failed to create/update TXT record: HTTP status {status_code}")

if __name__ == "__main__":
    main()
