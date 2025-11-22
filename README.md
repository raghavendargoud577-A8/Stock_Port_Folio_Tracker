# Stock_Port_Folio_Tracker

# Stock Portfolio Tracker
# -----------------------

# Hardcoded price list (feel free to edit)
prices = {
    "AAPL": 180,
    "TSLA": 250,
    "GOOGL": 1200,
    "MSFT": 300,
    "AMZN": 3400
}

def add_stock(portfolio):
    """Prompt user for a stock symbol and quantity, then add to the portfolio."""
    ticker = input("Enter stock symbol: ").upper()
    if ticker not in prices:
        print(f"⚠️  Price for {ticker} not found – skipping.")
        return

   try:
        qty = int(input(f"How many shares of {ticker} do you have? "))
    except ValueError:
        print("⚠️  Please enter a whole number.")
        return

   portfolio[ticker] = portfolio.get(ticker, 0) + qty
    print(f"✅  Added {qty} share(s) of {ticker}.\n")

def show_total(portfolio):
    """Calculate and display the total investment value."""
    total = sum(prices[t] * q for t, q in portfolio.items())
    print(f"📊  Total portfolio value: ${total:,.2f}")

def save_to_file(portfolio):
    """Write the portfolio (ticker,quantity) to a CSV file."""
    filename = input("Enter filename (e.g., portfolio.csv): ")
    try:
        with open(filename, "w") as f:
            f.write("Ticker,Quantity\n")
            for ticker, qty in portfolio.items():
                f.write(f"{ticker},{qty}\n")
        print(f"✅  Saved to {filename}")
    except OSError as e:
        print(f"⚠️  Could not save file: {e}")

def main():
    portfolio = {}
    actions = {
        "1": ("Add a stock", add_stock),
        "2": ("Show total investment", show_total),
        "3": ("Save portfolio to file", save_to_file),
        "0": ("Exit", lambda _: None)
    }

   while True:
        print("\n--- Stock Portfolio Tracker ---")
        for k, (desc, _) in actions.items():
            print(f"{k}. {desc}")
        choice = input("Choose an option: ")

  if choice not in actions:
            print("⚠️  Invalid choice – try again.")
            continue
     _, func = actions[choice]
        if choice == "0":
            print("👋  Bye!")
            break
        func(portfolio)

if __name__ == "__main__":
    main()
