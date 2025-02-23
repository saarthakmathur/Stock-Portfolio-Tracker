# Stock-Portfolio-Tracker

import yfinance as yf
import pandas as pd

class StockPortfolio:
    def __init__(self):
        self.portfolio = pd.DataFrame(columns=['Ticker', 'Shares', 'Purchase Price'])

    def add_stock(self, ticker, shares, purchase_price):
        new_stock = pd.DataFrame([[ticker, shares, purchase_price]], columns=['Ticker', 'Shares', 'Purchase Price'])
        self.portfolio = pd.concat([self.portfolio, new_stock], ignore_index=True)

    def view_portfolio(self):
        if self.portfolio.empty:
            print("Your portfolio is empty.")
        else:
            print("\nYour Stock Portfolio:")
            print(self.portfolio)

    def get_current_prices(self):
        if self.portfolio.empty:
            print("Your portfolio is empty.")
            return

        print("\nCurrent Stock Prices:")
        for ticker in self.portfolio['Ticker']:
            stock = yf.Ticker(ticker)
            current_price = stock.history(period='1d')['Close'].iloc[-1]
            print(f"{ticker}: ${current_price:.2f}")

    def calculate_total_value(self):
        total_value = 0
        for index, row in self.portfolio.iterrows():
            ticker = row['Ticker']
            shares = row['Shares']
            stock = yf.Ticker(ticker)
            current_price = stock.history(period='1d')['Close'].iloc[-1]
            total_value += shares * current_price
        return total_value

if __name__ == "__main__":
    tracker = StockPortfolio()

    while True:
        print("\nStock Portfolio Tracker")
        print("1. Add Stock")
        print("2. View Portfolio")
        print("3. Get Current Prices")
        print("4. Calculate Total Portfolio Value")
        print("5. Exit")

        choice = input("Choose an option: ")

        if choice == '1':
            ticker = input("Enter stock ticker: ")
            shares = int(input("Enter number of shares: "))
            purchase_price = float(input("Enter purchase price per share: "))
            tracker.add_stock(ticker, shares, purchase_price)
        elif choice == '2':
            tracker.view_portfolio()
        elif choice == '3':
            tracker.get_current_prices()
        elif choice == '4':
            total_value = tracker.calculate_total_value()
            print(f"\nTotal Portfolio Value: ${total_value:.2f}")
        elif choice == '5':
            print("Exiting the program.")
            break
        else:
            print("Invalid choice. Please try again.")
