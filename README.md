# Option Pricing Engine

A robust, object-oriented Python option pricing engine developed for the Quant Finance Toolkit Ecosystem at IIIT Hyderabad. This engine algorithmically routes and evaluates European, American, and multi-asset OTC derivatives using a unified API, ensuring strict adherence to theoretical pricing invariants, put-call parity, and absolute boundary constraints.

## Key Features
* **Dynamic API Routing:** A single `Pricer.price()` entry point automatically delegates pricing to the most efficient mathematical engine based on instrument style and market conditions.
* **Dividend Handling:** Supports continuous dividend yields and dynamic spot-price adjustments for discrete cash dividends across tree and grid boundaries.
* **High-Dimensional Pricing:** Overcomes the curse of dimensionality for multi-asset basket options using Monte Carlo simulations.
* **Early-Exercise Boundary Extraction:** Extracts and visualizes the critical free-boundary curve $S^*(t)$ where immediate intrinsic value equals the continuation value.
* **Real-World Calibration:** Empirically validated against historical AAPL, GOOGL, and TSLA options chains to analyze volatility skew and early-exercise premiums.

## Mathematical Framework

The engine implements three core computational models to cover the full spectrum of vanilla and exotic options:

1. **Black-Scholes-Merton (BSM):** 
   Provides closed-form analytical solutions for European options and American Calls on non-dividend-paying assets.
2. **Binomial Tree (Cox-Ross-Rubinstein):**
   A discrete-time recombining lattice model used to evaluate early-exercise premiums for American Puts and dividend-paying American Calls. It includes interpolation logic to handle discrete cash dividends while preventing arbitrage boundary violations.
3. **Least Squares Monte Carlo (LSMC):**
   A machine-learning-driven approach to solve the optimal stopping problem for high-dimensional derivatives (e.g., Average Basket Options). It uses cross-sectional ordinary least squares (OLS) regression against polynomial basis functions to approximate the conditional expectation of continuation, maintaining $\mathcal{O}(M)$ linear scaling for multi-asset contracts.

## Architecture

The system utilizes an immutable, `dataclass`-driven architecture for clean state management. 

```text
quantkit/
├── pricing/
│   ├── core/         # Instruments, MarketData, unified Pricer API
│   ├── engines/      # BSM, Binomial Tree, and LSMC mathematical engines
│   └── utils/        # Boundary extraction and visualization scripts
└── tests/            # pytest suite for invariants and unit tests
