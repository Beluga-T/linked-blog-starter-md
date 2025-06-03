# Astronomy 25 - Spring 2025 - Homework 6 Solutions

## Problem 1: Galaxy Mass (15 points)

**Given:**
- Both galaxies have radius R = 20 kpc
- Galaxy A rotation speed: v_A = 100 km/s
- Galaxy B rotation speed: v_B = 200 km/s

### a) Ratio of masses M_A to M_B

**Solution:**
For circular orbital motion in galaxies, we use the relationship between rotation speed and mass:
v² = GM/R

Since both galaxies have the same radius (R = 20 kpc), we can find the mass ratio:

For Galaxy A: v_A² = GM_A/R
For Galaxy B: v_B² = GM_B/R

Taking the ratio:
M_A/M_B = v_A²/v_B² = (100 km/s)²/(200 km/s)² = 10,000/40,000 = 1/4

**Answer: M_A/M_B = 0.25 or 1:4**

### b) Which galaxy is farther away?

**Solution:**
Since both galaxies have the same flux (apparent brightness) but different intrinsic properties, we need to consider their luminosities.

Galaxy B has 4 times more mass than Galaxy A, which typically correlates with higher luminosity. If Galaxy B is more luminous but appears equally bright as Galaxy A, then Galaxy B must be farther away.

Using the inverse square law: F = L/(4πd²)
If F_A = F_B but L_B > L_A, then d_B > d_A

**Answer: Galaxy B is farther away because it has higher mass (and likely higher luminosity) but the same apparent brightness as Galaxy A.**

### c) Total mass of Galaxy B in solar masses

**Solution:**
Using the circular velocity equation: M = v²R/G

Given:
- v_B = 200 km/s = 2.00 × 10⁵ m/s
- R = 20 kpc = 20 × 3.086 × 10¹⁹ m = 6.172 × 10²⁰ m
- G = 6.67408 × 10⁻¹¹ m³/(kg·s²)

M_B = (2.00 × 10⁵)² × (6.172 × 10²⁰) / (6.67408 × 10⁻¹¹)
M_B = 4.00 × 10¹⁰ × 6.172 × 10²⁰ / (6.67408 × 10⁻¹¹)
M_B = 3.70 × 10⁴¹ kg

Converting to solar masses (M_☉ = 1.989 × 10³⁰ kg):
M_B = 3.70 × 10⁴¹ / (1.989 × 10³⁰) = 1.86 × 10¹¹ M_☉

**Answer: M_B ≈ 1.9 × 10¹¹ solar masses**

## Problem 2: The Distance Ladder (10 points)

### a) Hubble's method for determining galactic distances

**Solution:**
Edwin Hubble used **Cepheid variable stars** as "standard candles" to determine distances to other galaxies, particularly the Andromeda galaxy (M31).

**Hubble's Method:**
1. **Identification**: Hubble identified Cepheid variable stars in M31 and other nearby galaxies
2. **Period-Luminosity Relationship**: He used Henrietta Swan Leavitt's discovery that Cepheids have a direct relationship between their pulsation period and intrinsic luminosity
3. **Distance Calculation**: By measuring the apparent brightness and period of Cepheids in distant galaxies, he could calculate their intrinsic luminosity and thus determine the distance using the inverse square law

**How it works**: 
- Measure the pulsation period of a Cepheid in a distant galaxy
- Use the period-luminosity relationship to determine its absolute magnitude
- Compare with its apparent magnitude to calculate distance using the distance modulus formula

### b) White dwarf supernovae for distant galaxies

**Solution:**
White dwarf supernovae (Type Ia supernovae) are useful for measuring distances to very distant galaxies because:

**Advantages:**
1. **Standard Candles**: Type Ia supernovae have remarkably consistent peak luminosities (~10¹⁰ L_☉), making them excellent standard candles
2. **Extreme Brightness**: They are among the brightest objects in the universe, visible across cosmological distances
3. **Consistent Physics**: They result from a white dwarf accreting matter until it reaches the Chandrasekhar limit (~1.4 M_☉), leading to consistent explosion energies

**Limitations**: They cannot be used to find the distance to any galaxy you want because:
- They are rare events (occur roughly once per century per galaxy)
- You must wait for one to occur in your target galaxy
- They are transient events lasting only weeks to months

## Problem 3: Different Types of Galaxies (10 points)

### a) Structural differences between galaxy types

**Solution:**

**Spiral Galaxies:**
- Distinct spiral arm structure with a central bulge and extended disk
- Active star formation in spiral arms creates blue, luminous regions
- Contains significant amounts of gas and dust
- Well-organized rotation with differential rotation patterns

**Elliptical Galaxies:**
- Smooth, featureless elliptical shape with no spiral structure
- Dominated by older, redder stars with little ongoing star formation
- Minimal gas and dust content
- Stars follow more random orbital patterns rather than organized rotation

**Irregular Galaxies:**
- Lack organized structure of spirals or smooth profile of ellipticals
- Often show chaotic, asymmetric shapes
- Typically smaller than spirals and ellipticals
- Can have active star formation but in a disorganized pattern

### b) Location of young stars

**Solution:**
Young stars are primarily found in **spiral galaxies**, specifically in the spiral arms where active star formation occurs due to density waves compressing gas and dust. Some young stars can also be found in **irregular galaxies** where gas compression and star formation can occur, though less organized than in spirals.

Elliptical galaxies contain very few young stars since they have exhausted most of their gas and dust needed for star formation.

## Problem 4: Hubble's Law (15 points)

**Given:**
- Rest wavelength of hydrogen line: λ₀ = 656 nm
- Galaxy 1 observed wavelength: λ₁ = 668 nm  
- Galaxy 2 observed wavelength: λ₂ = 680 nm

### a) Calculate redshift z for each galaxy

**Solution:**
Redshift is defined as: z = (λ_observed - λ_rest)/λ_rest = Δλ/λ₀

**Galaxy 1:**
z₁ = (668 - 656)/656 = 12/656 = 0.0183

**Galaxy 2:**
z₂ = (680 - 656)/656 = 24/656 = 0.0366

### b) Recessional velocity of Galaxy 1

**Solution:**
For small redshifts (z << 1), we can use the approximation:
v = c × z

Where c = 3.00 × 10⁵ km/s

v₁ = c × z₁ = (3.00 × 10⁵ km/s) × 0.0183 = 5,490 km/s

**Answer: v₁ ≈ 5,490 km/s**

### c) Distance comparison between galaxies

**Solution:**
From Hubble's Law: v = H₀ × d, so distance is proportional to velocity
Since v ∝ z for small redshifts: d ∝ z

The ratio of distances is:
d₂/d₁ = z₂/z₁ = 0.0366/0.0183 = 2.00

**Answer: Galaxy 2 is 2.0 times farther away than Galaxy 1**

---

## Key Formulas Used:
- Circular orbital velocity: v² = GM/R
- Redshift: z = Δλ/λ₀
- Hubble's Law: v = H₀d
- Distance modulus: m - M = 5 log(d) - 5
- Inverse square law: F = L/(4πd²)