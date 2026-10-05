import time
import requests

# === CONFIGURATION ===
# 1. Twilio Credentials (Get these free from twilio.com)
TWILIO_ACCOUNT_SID = "ACb05b6f71cf761f862da5bad106af21f0"
TWILIO_AUTH_TOKEN = "b3f6c9ad78cf5d8caaef51c291cba469"
TWILIO_PHONE_NUMBER = "+17372508034"  # Your Twilio virtual number

# 2. Your Personal Target Number
MY_IPHONE_NUMBER = "+16478680247"  # e.g., "+1234567890"

# 3. Third-party Kick Data API (e.g., ScrapeCreators or Parse.bot)
KICK_API_URL = "https://api.scrapecreators.com/v1/kick/profile?handle=punchmade"
API_KEY = "abpfqVoHRBTLsB5v6QlxZEFJMq02"

# Track state so you only get ONE SMS per live event
is_live = False

def check_kick_stream():
    global is_live
    headers = {"x-api-key": API_KEY}
    
    try:
        response = requests.get(KICK_API_URL, headers=headers).json()
        # Evaluates if the livestream metadata exists
        currently_live = response.get("livestream") is not None 
        
        if currently_live and not is_live:
            send_sms_to_iphone("🚨 Alert: punchmade is officially LIVE on Kick! kick.com/punchmade")
            is_live = True
        elif not currently_live:
            is_live = False  # Reset state when they go offline
            
    except Exception as e:
        print(f"Error checking status: {e}")

def send_sms_to_iphone(message_body):
    twilio_url = f"https://api.twilio.com/2010-04-01/Accounts/{TWILIO_ACCOUNT_SID}/Messages.json"
    payload = {
        "To": MY_IPHONE_NUMBER,
        "From": TWILIO_PHONE_NUMBER,
        "Body": message_body
    }
    
    try:
        response = requests.post(twilio_url, data=payload, auth=(TWILIO_ACCOUNT_SID, TWILIO_AUTH_TOKEN))
        if response.status_code == 201:
            print("SMS successfully sent to your iPhone!")
        else:
            print(f"Failed to send SMS: {response.text}")
    except Exception as e:
        print(f"Twilio API Error: {e}")

# Run loop every 3 minutes
print("Monitoring kick.com/punchmade for live status...")
while True:
    check_kick_stream()
    time.sleep(25)
