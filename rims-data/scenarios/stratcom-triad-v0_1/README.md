# STRATCOM Triad Synthetic Demo v0.1

This scenario is synthetic, unclassified demo data. Names are fictional
composites, dates are illustrative, and values are designed to exercise R-IMS
portfolio modernization mechanics rather than describe real force structure or
program schedules.

## Demo Script

The live talking track for this scenario is maintained in
[demo-script.md](demo-script.md). Keep scenario-specific story, data
checkpoints, caveats, and operator notes there so they survive OpenSpec change
archive moves.

The scenario intentionally reuses the existing portfolio shell and legacy view
file names. Domain semantics are encoded through the current portfolio abstract
fields:

- `region`: deterrence leg
- `category`: platform type
- `capacity_mw`: synthetic deterrence availability points (DAP)
- `nameplate_mw`: synthetic planned DAP
- `delta_capacity`: synthetic DAP change mapped by the STRATCOM converter adapter

DAP is a normalized demo score used to make portfolio timing visible. It is not
a warhead count, launcher count, sortie-generation measure, operational exchange
ratio, or claim of substitutability between triad legs. The scenario floor is
kept coherent across `load_forecast_by_region.json` and
`portfolio_abstract.json` so tests and charts use the same modeled requirement.

The DAP ratios are intentionally portfolio-score ratios, not force-structure
ratios. Public-source framing treats the triad legs as complementary and
describes the sea-based leg as the most survivable pillar, so the demo should
not be read as saying that a low sea DAP floor means low strategic value. See:

The current score mix keeps land as the largest modernization exposure, raises
air sustainment enough for the baseline portfolio to clear the modeled floor,
and makes legacy bomber drawdown timing the available bridge after the larger
legacy ICBM drawdown delay is blocked. This is synthetic portfolio scoring and
does not imply force substitutability.

- DoD, America's Nuclear Triad:
  https://www.defense.gov/serve-from-netstorage/Experience/Americas-Nuclear-Triad/index.html
- CRS, Defense Primer: Strategic Nuclear Forces:
  https://www.congress.gov/crs-product/IF10519

Do not add classified, FOUO, CUI, or operationally sensitive data to this
scenario folder.

## Demo Script

- [STRATCOM demo script](demo-script.md)
