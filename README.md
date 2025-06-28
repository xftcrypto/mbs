# mbs


- USDX: 0x421C76cd7C1550c4fcc974F4d74c870150c45995



**Setup:**
- Underlying: $1M real estate portfolio (or loan pool, bonds, etc.)
- Payment currency: USDX
- Tranches: Senior/Mezz/Junior claims on the real estate

**User Flow:**

1. **Deposit**: Alice pays 1000 USDX → gets 1000 tranche tokens (representing claim on underlying real estate)
2. **Interest**: Real estate generates income → Alice gets paid interest in USDX
3. **Losses**: If real estate loses value → admin calls `allocateLosses()`
4. **Redeem**: At maturity → Alice gets USDX back based on remaining real estate value

**Example:**
- Real estate worth $1M initially
- Alice deposits 1000 USDX for senior tranche tokens
- Real estate drops to $800K (20% loss)
- Junior tranche absorbs the loss first
- Alice (senior) still gets most of her 1000 USDX back

**Key Point:**
- USDX = **payment rail** (like using dollars)
- Tranche tokens = **ownership claim** on the real underlying asset
- Losses come from the **underlying asset performance**, not USDX

The contract holds USDX as collateral to back the claims, but the actual investment risk is in whatever underlying asset the tranche represents.



---

**What Happened:**
- Deployed Senior Tranche (STT) on Sepolia
- 5% interest, 1-year maturity
- Uses USDX for payments
- Contract address: `0x8E970DE4f57A2Db6902C76D514D44BE245C97f38`

**Real MBS Simulation:**

**Setup 3 Tranches:**
1. **Senior AAA** (70%): 3% interest, safest
2. **Mezzanine BBB** (20%): 7% interest  
3. **Junior/Equity** (10%): 15% interest, riskiest

**Underlying:** $100M mortgage pool

**Flow:**
1. **Deposit Phase:** Investors deposit USDX into tranches
2. **Cash Flow:** Monthly mortgage payments → interest paid in USDX
3. **Defaults:** When mortgages default → admin calls `allocateLosses()`
4. **Loss Waterfall:** 
   - Junior absorbs first 10% losses
   - Mezzanine absorbs next 20% losses  
   - Senior only loses if >30% total defaults

**Example Loss Scenario:**
- $20M mortgage defaults (20% loss)
- Junior: Wiped out ($10M loss)
- Mezzanine: Takes $10M hit
- Senior: Gets full principal back

**Code Implementation:**
```javascript
// Deploy 3 tranches with different rates/seniority
// Admin periodically calls allocateLosses() based on mortgage defaults
// Interest payments simulate mortgage cash flows
```

This creates a synthetic MBS where USDX flows represent mortgage cash flows and losses.
