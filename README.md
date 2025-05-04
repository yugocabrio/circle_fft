# Circle FFT

This educational notebook demonstrates Circle FFT over the finite field $\mathbb{F}_{31}$(Mersenne 5, $p=2^5 -1$).

## Overview

Circle FFT is a specialized FFT algorithm that operates on the circle group $x^2 + y^2 = 1$ over finite fields. Unlike classical multiplicative FFTs that require $p-1$ to be smooth, Circle FFT only requires $p+1$ to have large powers of 2 as factors. This implementation uses $\mathbb{F}_{31}$ as the base field, which is a CFFT-friendly Mersenne prime ($p \equiv 3 \bmod 4$) with $p+1 = 32 = 2^5$.

The Circle FFT transform maps evaluations on specially constructed domains (standard position cosets) to polynomial coefficients in a basis that spans a space of dimension $N$ (one less than the traditional polynomial space).

## Contents

1. **Finite Field Element Class**
   - Complete arithmetic operations in $\mathbb{F}_{31}$
   - Addition, subtraction, multiplication, division
   - Exponentiation and inverse calculation using Fermat's Little Theorem

2. **Circle Point Class**
   - Points on the circle $x^2 + y^2 = 1$ over $\mathbb{F}_{31}$
   - Group operation implementation: $(x_1,y_1) \cdot (x_2,y_2) = (x_1x_2 - y_1y_2, x_1y_2 + x_2y_1)$
   - Squaring map: $\pi(x,y) = (2x^2-1, 2xy)$
   - Finding generators and computing powers

3. **Subgroup Generation**
   - Generate subgroups $G_n$ of size $2^n$ from the circle group
   - Demonstration of the group structure

4. **Coset Generation**
   - Twin cosets: $D = Q \cdot G_{n-1} \cup Q^{-1} \cdot G_{n-1}$
   - Standard position cosets: $D = Q \cdot G_n = Q \cdot G_{n-1} \cup Q^{-1} \cdot G_{n-1}$
   - Decomposition of cosets and squaring map halving

5. **Circle FFT Algorithm**
   - Transform evaluations to coefficients in $L'_N$ space

6. **Circle IFFT Algorithm**
   - Inverse transform from coefficients to evaluations

7. **Verification**
   - Basis function checks using the FFT basis $B_n$
   - End-to-end verification of FFT and IFFT operations

## Mathematical Background

The Circle FFT algorithm leverages properties of the circle group $C(\mathbb{F}_p) = \{(x,y) \in \mathbb{F}_p \times \mathbb{F}_p : x^2 + y^2 = 1\}$ to perform efficient transformations.

### Key Components

- **Circle Group**: The circle $x^2 + y^2 = 1$ forms a group under the operation $(x_1,y_1) \cdot (x_2,y_2) = (x_1x_2 - y_1y_2, x_1y_2 + x_2y_1)$
- **Squaring Map**: The map $\pi(x,y) = (2x^2-1, 2xy)$ is "2-to-1" and enables the divide-and-conquer approach
- **Involution**: The map $J(x,y) = (x,-y)$ which helps split domains into even/odd components
- **FFT Basis**: $B_n = \{b_j^{(n)}(x,y) = y^{j_0}v_1(x)^{j_1}...v_{n-1}(x)^{j_{n-1}} \mid j = \sum_{i=0}^{n-1}j_i2^i\}$

## Usage Example

```python
Q = CirclePoint(FieldElement(24), FieldElement(18))
Domain = generate_standard_position_coset(Q, generate_subgroup(3))

evals = [1, 2, 4, 8, 16, 1, 2, 4]

coeffs = circle_fft(evals, Domain)
print(f"FFT coefficients: {coeffs}")

reconstructed = circle_ifft(coeffs, Domain)
print(f"Reconstructed: {reconstructed}")
```

## References

- Circle STARKs – U. Haböck, D. Levit, S. Papini  
  <https://eprint.iacr.org/2024/278>
- Exploring Circle STARKs – V. Buterin  
  <https://vitalik.eth.limo/general/2024/07/23/circlestarks.html>
  <https://github.com/ethereum/research/tree/master/circlestark>
- Plonky3 implementation  
  <https://github.com/Plonky3/Plonky3>
- Rust demo of Circle FFT – Coset  
  <https://github.com/0xxEric/CircleFFT>
