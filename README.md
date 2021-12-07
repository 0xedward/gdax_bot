# gdax_bot fork

Fork of [@kdmukai](https://github.com/kdmukai)'s [gdax_bot](https://github.com/kdmukai/gdax_bot) with the following changes:
1. AWS SNS replaced with Pushover and added `--push_notify` option to send push notifications with Pushover
2. Replace storing secrets in `settings.conf` with use of environment variables
3. `--debug` option logs all HTTP requests for auditing and debugging

For additional information, refer to @kdmukai's [documentation](https://github.com/kdmukai/gdax_bot#readme)

## Setup
1. Create a Coinbase account
2. Clone the repo and create a virtualenv
```bash
python3 -m venv gdax_bot_env
```
3. Activate your newly created virtualenv
```bash
source gdax_bot_env/bin/activate
```
4. Install dependencies
```bash
cd gdax_bot && pip install -r requirements.txt
```
5. *[Optional] Refer to @kdmukai's [documentation](https://github.com/kdmukai/gdax_bot#readme), if you want to set up a dry run of the bot on Coinbase Pro's sandbox environment*
6. *[Optional] Create an [Pushover](https://pushover.net/) account to get push notifications whenever the bot is run*
7. *[Optional] [Create an application token on Pushover](https://pushover.net/apps/build) and copy your application token and user key*
8. *[Optional] Set up environment variables for Pushover*
```bash
export PUSHOVER_APP_TOKEN="paste your application token here"
export PUSHOVER_USER_KEY="paste your Pushover User key here"
```
9. Set up environment variables for your Coinbase Pro
```bash
export COINBASE_PROD_API_KEY="paste your api key here"
export COINBASE_PROD_PASSPHRASE="paste your api passphrase here"
export COINBASE_PROD_API_SECRET_KEY="paste your api secret key (it should be some string in base64) here"
```
10. Schedule the bot to run as a cron job
Suppose you want to buy €75 EUR of BTC every other day at 14:00, if you want to get push notifications and have set up Pushover, run `crontab -e` and add the following to your cronjobs:
```
23 17 * * 1 . /home/username/.profile; /path/to/gdax_bot_env/bin/python -u /path/to/repo/gdax_bot/gdax_bot.py -j ETH-USD BUY 50.00 USD --push_notify >> /path/to/coinbase-orders.log
```

If you don't want to use Pushover, exclude the `--push_notify` option
```
23 17 * * 1 . /home/username/.profile; /path/to/gdax_bot_env/bin/python -u /path/to/repo/gdax_bot/gdax_bot.py -j ETH-USD BUY 50.00 USD >> /path/to/coinbase-orders.log
```

11. *[Optional] If you are using WSL, you may want to take the [extra step to ensure cron service starts with Windows](https://www.howtogeek.com/746532/how-to-launch-cron-automatically-in-wsl-on-windows-10-and-11/)*
