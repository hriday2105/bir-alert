# Bir Alert: early warning for desert wells

Hackathon entry for **W-001: Early Warnings for Desert Water Sources**.

Bir Alert predicts how many days a well has left, says how sure it is, warns the village by SMS in plain language, and sends each at-risk village to a **different** backup well so a warning doesn't cause a stampede to one source. (*Bir* is Arabic for well.)

**Live demo:** open `index.html` in any browser, or use the GitHub Pages link: `https://YOUR-USERNAME.github.io/bir-alert/`

## The problem inside the problem

If you tell everyone that Well A is failing, every herd walks to Well B and kills it too. Most early-warning systems stop at the alert. Bir Alert also balances the response, the way a traffic app avoids sending every car down the same road.

## What the demo shows

| Hackathon requirement | Where in the demo |
|---|---|
| Data about water sources | 24 wells in a Marsabit-style region, a year of daily readings |
| Prediction with confidence | "18 to 31 days left" with an 80% range, plus a best guess |
| Warning weeks before the crisis | Watch, Warning and Act-now tiers, with a "first alert, N weeks early" marker per well |
| Sensor dies | Checkbox: falls back to the regional trend of neighbouring wells, widens the range, tags the SMS "lower confidence" |
| Cloudy satellite image | Checkbox: widens the range again and notes a Landsat or radar fallback |
| False alarm vs missed alarm | Slider for how much worse a miss is than a false alarm; the backtest panel shows catches, lead time and false-alarm wells |
| Basic phones | SMS mockup in English or Swahili; herders reply 1, 2 or 3 and the forecast updates |
| Our angle | Routing panel compares nearest-well routing against load-balanced routing and counts overloaded backup wells |

## Real vs simulated data (honest labelling)

| Item | Status in this demo |
|---|---|
| Rainfall and heat series | **Simulated** drought-year series |
| Well levels and sensor readings | **Simulated** by a water-balance model |
| Herd sizes and village positions | **Simulated** |
| Well names | Labels only; no claim of real well data |

The backtest numbers shown in the app are computed on this simulated data, not on real wells.

## How it works

1. Each day the model fits a 14-day trend to the sensor readings and projects when the level reaches zero.
2. The slope uncertainty gives an 80% range. The tier is chosen from the lower bound, so uncertainty makes alerts earlier, not later.
3. The cost slider moves the alert horizon: the higher a miss costs, the earlier it warns and the more false alarms it accepts.
4. At-risk villages are assigned to healthy wells by distance plus a heavy penalty for exceeding a well's estimated capacity.

## Path to real data

The same pipeline is designed to take real inputs: CHIRPS rainfall, ERA5 evaporation (for example via the Open-Meteo archive) and Sentinel-2 NDWI or NDVI for ponds and oases, which give real dry-down events to backtest against. This is the next step and is not implemented in the current demo.

## Deployment idea

A county water authority or aid agency runs it. Wells without sensors use satellite and rainfall only, with wider ranges. Alerts go out by SMS (a cost of a fraction of a cent each, no data plan), and the Act-now list tells the agency which wells to send water trucks to first.

## Limitations

- Simulated data; the numbers show the method works, not field accuracy.
- Rolling-trend forecasts assume steady drawdown. A real version should add a learned model with calibrated intervals.
- Live SMS sending is mocked, not connected to a provider.

## Run locally

No install needed. Open `index.html` in a browser.
