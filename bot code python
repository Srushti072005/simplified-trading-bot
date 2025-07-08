import logging
from binance.client import Client
from binance.enums import *
import os

# Setup logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

class BasicBot:
    def _init_(self, api_key, api_secret, testnet=True):
        if testnet:
            self.client = Client(api_key, api_secret, testnet=True)
            self.client.FUTURES_URL = 'https://testnet.binancefuture.com/fapi'
        else:
            self.client = Client(api_key, api_secret)

    def place_order(self, symbol, side, order_type, quantity, price=None):
        try:
            if order_type == 'MARKET':
                order = self.client.futures_create_order(
                    symbol=symbol,
                    side=side,
                    type=ORDER_TYPE_MARKET,
                    quantity=quantity
                )
            elif order_type == 'LIMIT':
                order = self.client.futures_create_order(
                    symbol=symbol,
                    side=side,
                    type=ORDER_TYPE_LIMIT,
                    timeInForce=TIME_IN_FORCE_GTC,
                    quantity=quantity,
                    price=price
                )
            else:
                raise ValueError("Unsupported order type")

            logging.info(f"Order placed: {order}")
            return order

        except Exception as e:
            logging.error(f"Error placing order: {e}")
            return None

    def get_account_info(self):
        try:
            info = self.client.futures_account()
            logging.info("Fetched account info")
            return info
        except Exception as e:
            logging.error(f"Failed to fetch account info: {e}")
            return None

# CLI Interface
def main():
    api_key = input("Enter your Binance API Key: ")
    api_secret = input("Enter your Binance API Secret: ")

    bot = BasicBot(api_key, api_secret)

    symbol = input("Enter trading pair (e.g., BTCUSDT): ").upper()
    side = input("Enter side (BUY or SELL): ").upper()
    order_type = input("Enter order type (MARKET or LIMIT): ").upper()
    quantity = float(input("Enter quantity: "))
    price = None
    if order_type == 'LIMIT':
        price = input("Enter price: ")

    order = bot.place_order(symbol, side, order_type, quantity, price)

    if order:
        print("Order executed successfully.")
    else:
        print("Order failed. Check logs for details.")

if _name_ == "_main_":
    main()
