# Six Nations 2027 forecast

**Live site:** https://naoiseirwin.github.io/six-nations-2027/

A probabilistic forecast of the 2027 Men's Six Nations Championship: each team's chance of winning the title, a Grand Slam, the Triple Crown or the wooden spoon, of finishing in each position, and of winning each of the 15 matches. Forecast as of 4 Nov 2026, using results through 31 Oct 2026.

This is a personal project for fun and interest. It is not betting advice.

## Headline odds (as of 4 Nov 2026)

| Team | Title | Grand Slam | Triple Crown | Wooden spoon | Bookmaker implied title (benchmark)* |
|---|---|---|---|---|---|
| Ireland | 37.3% | 16.9% | 32.0% | 0.4% | 24.8% |
| France | 30.1% | 11.0% | n/a | 0.3% | 39.1% |
| England | 21.9% | 7.4% | 15.6% | 0.7% | 23.2% |
| Scotland | 10.4% | 2.5% | 11.8% | 1.4% | 10.9% |
| Italy | 0.1% | <0.1% | n/a | 43.5% | 1.0% |
| Wales | 0.1% | <0.1% | 0.2% | 53.8% | 0.9% |

*Benchmark only: OLBG 'Win Tournament' odds table, quoted 4 Nov 2026, 9:10 AM ET, converted to probabilities and normalised over the six teams. Shown for comparison only; odds are never an input to the model. Triple Crown applies to England, Ireland, Scotland and Wales only.

## How the model works

- **Team strength.** Each team's strength is a 50/50 blend of two things: our own rating, built from every men's Test result since 2000, and the official World Rugby rankings.
- **Matches.** A strength gap, plus a home advantage of about 4.3 points estimated from past Six Nations matches, turns into a probability of winning and an expected winning margin for each match.
- **Championship.** The whole championship is simulated 100,000 times (random seed 0) under the official rules: 4 points for a win and 2 for a draw, a bonus point for scoring 4 or more tries, a bonus point for losing by 7 or fewer, and 3 extra points for a Grand Slam. Ties go to points difference, then tries. Each team's odds are the share of simulations in which it happens.
- **No peeking.** Model settings were fixed ("frozen") using pre-2019 data only before any test was scored, and the rule for picking the published version was written down in advance. Each backtest uses only data available before that championship's opening match.

## How well it has done (backtests)

The same model, with the same frozen settings, was run on every Six Nations from 2019 to 2026 as if forecasting it beforehand: 120 matches in all. Lower scores are better.

**Match predictions (log loss)**

| | 2019 | 2020 | 2021 | 2022 | 2023 | 2024 | 2025 | 2026 | Mean |
|---|---|---|---|---|---|---|---|---|---|
| This model | **.427** | .512 | .682 | .615 | .326 | .656 | .345 | .564 | **.516** |
| Our ratings alone | .445 | **.481** | **.646** | .667 | .383 | .728 | .376 | **.520** | .531 |
| World Rugby rankings alone | .436 | .536 | .700 | **.576** | .334 | **.604** | .361 | .610 | .520 |
| World Rugby's simple prediction formula | .455 | .594 | .922 | .679 | **.268** | .683 | **.294** | .614 | .563 |

Bold marks the best model each year. The blend had the best average and was the published choice, but it was the best in only 1 of the 8 years.

**Chance given to the eventual champion beforehand**

| | 2019 Wales | 2020 England | 2021 Wales | 2022 France | 2023 Ireland | 2024 Ireland | 2025 France | 2026 France |
|---|---|---|---|---|---|---|---|---|
| This model | 18.9% | 61.1% | 3.3% | 19.3% | 55.1% | 54.8% | 23.4% | 33.9% |
| Our ratings alone | 13.1% | 62.3% | 8.2% | 16.5% | 48.4% | 54.0% | 17.9% | 36.8% |
| World Rugby rankings alone | 23.4% | 55.4% | 2.2% | 21.6% | 51.7% | 52.2% | 28.3% | 29.3% |
| World Rugby's formula | 21.8% | 80.4% | 0.3% | 14.9% | 75.8% | 68.8% | 15.5% | 25.4% |
| Bookmakers (benchmark) | not found | 50.4% | 6.8% | not found | partial | 35.0% | not found | not found |

**Where it lost**

- On match predictions, World Rugby's simple formula beat the model in 2023 and 2025; our ratings alone beat it in 2020, 2021 and 2026; the rankings alone beat it in 2022 and 2024.
- On picking the champion, the formula did better in 2019, 2020, 2023 and 2024 (4 of 8 years).
- Bookmaker odds from before the opener could only be found for 2020, 2021 and 2024. Over those three years the bookmakers scored better on the champion by log loss (1.473 vs 1.497); the model scored better by Brier score (0.105 vs 0.117). In 2021 the bookmakers gave champions Wales 6.8% and we gave them 3.3%.
- The lowest chances it gave the eventual champion were Wales in 2021 (3.3%), Wales in 2019 (18.9%) and France in 2022 (19.3%).
- Favourites given 70–80% (23 matches, 74.7% on average) won only 60.9% of the time, so the model was overconfident in that range.

## Caveats and items to confirm

- 2027 kick-off times are as published on 4 Nov 2026 and may change for broadcast.
- Squad selection, injuries, travel, and the November 2026 Tests still to be played are not modelled. The forecast will move after those Tests.
- Exact ties on points, points difference and tries are "placed equally" under the rules. The model shares such places at random; this is rare.
- 2020 (four matches postponed to October by COVID) and 2021 (no crowds) are scored like any other year, and home advantage was kept for 2021.
- Some older World Rugby stats records omit try counts. A missing count is read as zero only when the score could be made without a try.
- World Rugby's ranking snapshot dates before 2020 are unreliably labelled.
- Eight championships is a small test sample, and only three of them have usable bookmaker odds.

## About this repo

`index.html` is the whole site: one self-contained file with no network requests, light and dark mode, and mobile friendly. It is generated from a private forecasting framework, which is not included here.

## Sources

- World Rugby match API (fixtures and results): https://api.wr-rims-prod.pulselive.com/rugby/v3/match?startDate=YYYY-01-01&endDate=YYYY-12-31&sport=mru (dated n/a; 2026–2027 accessed 4 Nov 2026, 9:03 AM ET; earlier years accessed 3 Nov 2026, 2:57–3:05 PM ET)
- World Rugby men's rankings API: https://api.wr-rims-prod.pulselive.com/rugby/v3/rankings/mru?date=YYYY-MM-DD (dated n/a; accessed 4 Nov 2026, 9:03 AM ET; the Monday before each Six Nations opener 2004–2026 and the Monday after, plus 2 and 4 Nov 2026)
- World Rugby match stats API (tries per team): https://api.wr-rims-prod.pulselive.com/rugby/v3/match/{id}/stats (dated n/a; accessed 4 Nov 2026, 9:03 AM–9:12 AM ET)
- Six Nations Rugby, Men's Six Nations fixtures: https://www.sixnationsrugby.com/en/m6n/fixtures (dated n/a; accessed 4 Nov 2026, 9:02 AM ET)
- Six Nations Rugby, Men's Six Nations 2027 fixtures and schedule confirmed: https://www.sixnationsrugby.com/en/m6n/news/mens-six-nations-2027-fixtures-and-schedule-confirmed (dated 9 Mar 2026, 4:27 AM ET; accessed 4 Nov 2026, 9:02 AM ET)
- Six Nations Rugby, Fixtures round-by-round guide to every match of the 2027 Six Nations: https://www.sixnationsrugby.com/en/m6n/news/fixtures-round-by-round-guide-to-every-match-2027-six-nations (dated n/a; accessed 4 Nov 2026, 9:02 AM ET)
- Six Nations Rugby, Championship rules: https://www.sixnationsrugby.com/en/m6n/championship-rules (dated n/a; accessed 4 Nov 2026, 9:02 AM ET)
- Six Nations Rugby, How does the Six Nations bonus points system work?: https://www.sixnationsrugby.com/en/m6n/news/how-does-the-six-nations-bonus-points-system-work (dated n/a; accessed 4 Nov 2026, 9:02 AM ET)
- OLBG, Six Nations betting preview ('Win Tournament' odds table, benchmark only): https://www.olbg.com/blogs/six-nations-betting-preview (dated 4 Nov 2026, 9:10 AM ET as quoted; accessed 4 Nov 2026, 9:10 AM ET)
- Oddschecker Six Nations winner odds, archived by the Wayback Machine (2020 benchmark only): https://web.archive.org/web/20200130213447/https://www.oddschecker.com/rugby-union/six-nations/six-nations-2020/winner (dated 30 Jan 2020, 4:34 PM ET; accessed 4 Nov 2026, 9:14 AM ET)
- Oddschecker Six Nations winner odds, archived by the Wayback Machine (2021 benchmark only): https://web.archive.org/web/20210130101404/https://www.oddschecker.com/rugby-union/six-nations/six-nations-2021/winner (dated 30 Jan 2021, 5:14 AM ET; accessed 4 Nov 2026, 9:14 AM ET)
- Oddschecker Six Nations winner odds, archived by the Wayback Machine (2024 benchmark only): https://web.archive.org/web/20240130002311/https://www.oddschecker.com/rugby-union/six-nations/winner (dated 29 Jan 2024, 7:23 PM ET; accessed 4 Nov 2026, 9:14 AM ET)
- odds1x2 Six Nations odds, archived by the Wayback Machine (top three prices only, not scored) (2023 benchmark only): https://web.archive.org/web/20221228112714/https://www.odds1x2.com/rugby-union/six-nations (dated 28 Dec 2022, 6:27 AM ET; accessed 4 Nov 2026, 9:14 AM ET)
