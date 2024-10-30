# main.py
from flask import Flask, request
from twilio.twiml.messaging_response import MessagingResponse
from datetime import datetime
import os
from replit import db

app = Flask(__name__)

# Simple database structure
PRICES = {
    'rice': {'makola': 450, 'kaneshie': 420, 'madina': 400},
    'tomatoes': {'makola': 15, 'kaneshie': 12, 'madina': 10},
    'cooking_oil': {'makola': 65, 'kaneshie': 60, 'madina': 58}
}

SUBSCRIBERS = {}

def check_subscription(phone):
    return phone in SUBSCRIBERS

@app.route('/webhook', methods=['POST'])
def webhook():
    incoming_msg = request.values.get('Body', '').lower()
    sender = request.values.get('From', '')
    
    resp = MessagingResponse()
    msg = resp.message()
    
    # New user
    if sender not in SUBSCRIBERS:
        payment_link = "https://paystack.com/pay/your-payment-link"
        msg.body(f"""Welcome to PriceAlert Ghana! 🇬🇭
        
Subscribe for only GHC 10/month to access:
- Daily price updates
- Market comparisons
- Best deal alerts

Pay here: {payment_link}

After payment, send your receipt number to activate.""")
        return str(resp)
    
    # Handle commands
    if 'price' in incoming_msg:
        item = incoming_msg.replace('price', '').strip()
        if item in PRICES:
            response = f"Current prices for {item}:\n\n"
            for market, price in PRICES[item].items():
                response += f"{market.title()}: GHC {price}\n"
        else:
            response = "Sorry, we don't have prices for that item yet. Try: rice, tomatoes, or cooking_oil"
    else:
        response = """Available commands:
- Type 'price rice' for rice prices
- Type 'price tomatoes' for tomato prices
- Type 'price cooking_oil' for cooking oil prices"""
    
    msg.body(response)
    return str(resp)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=8080)
