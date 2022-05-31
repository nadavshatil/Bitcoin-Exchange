# Bitcoin-Exchange
from flask import Flask, render_template, request
from coinbase.wallet.client import Client
import json
import requests
from datetime import datetime, timedelta
import matplotlib.pyplot as plt
import matplotlib.animation as animation
import mpld3


app = Flask(__name__)
app.config['SECRET_KEY'] = 'changeme'
API_Key = 'uZnnEWHf82krf5jP'
API_Secret = 'weEOpG0j7lPMy1GCrJbnF8JZh0Wi2lcY'
client = Client(API_Key, API_Secret)
start = datetime.now()
bitcoin_last_30_days_prices = []
dates = []
for day in range(1, 30):
    date = start-timedelta(days=day)
    price_data = client.get_spot_price(currency_pair='BTC-USD', date=date)
    price = price_data['amount']
    bitcoin_last_30_days_prices.append(price)
    dates.append(date.strftime("%Y-%m-%d"))
print(dates)
print(bitcoin_last_30_days_prices)
fig = plt.figure()
ax = fig.add_subplot(1, 1, 1)
ys = []
xs = []

def animate(i, xs, ys):
    ys = [price for price in range(10000, 100000, 500)]
    xs = dates
    ys = ys[:-20]
    xs = xs[:-20]
    ax.clear()
    ax.plot(xs, ys)
    plt.xticks(rotation=45, ha='right')
    plt.subplots_adjust(bottom=0.30)
    plt.title('BTC Price last 30 Days')
    plt.ylabel('USD$')
    plt.xlabel('Dates')


ani = animation.FuncAnimation(fig, animate, fargs=(xs, ys), interval=1000)
btc_graph = mpld3.fig_to_html(ani)



@app.route("/", methods=['GET', 'POST'])
def home():
    x = btc_graph
    return render_template("home.html", x=x)


# Press the green button in the gutter to run the script.
if __name__ == '__main__':
    app.run(host="0.0.0.0", port=8080, debug=True)

# See PyCharm help at https://www.jetbrains.com/help/pycharm/
© 2022 GitHub, Inc.
Terms
Privacy
Security
Status
Docs
Contact GitHub
Pricing
API
Training
Blog
About

