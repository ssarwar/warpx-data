# O₂ rotational angular model

This specifies the rotational outcome DCS for the combined vibrationally
elastic family. The complete derivation, equations and code mapping are in
[WarpX’s rotational-DCS specification](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering/rotational_dcs.rst).
[The development guide](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering/development.rst)
provides the workflow for continuing in another task or checkout.

## Definitions and applicability

Use E in eV, DCS in m²/sr, $P(E)=\sqrt{E(E+2m_ec^2)}$,
$y=\sin(\theta/2)$ and $\mu=\cos\theta$.
$I(E,\theta)=\sigma_{\rm IAA}(E)F(E,\theta)$ is the normalized elastic angular
shape times the IAA residual integral. It is rotationally inclusive, not an
extra rotationally unchanged channel.

The code’s common J index denotes O₂ nuclear rotation N; electron spin is
unresolved. Only odd N occurs, with statistical weight $g_N=2N+1$.
The bath populations are $b_N\propto g_Ne^{-\epsilon_N/k_BT}$, and

$$\epsilon_N=B[N(N+1)-2],\quad
B=0.00017828927734694198\ {
m eV},\quad
C_{ifL}=|\langle i0,L0\mid f0\rangle|^2.$$

Nonzero tensor ranks can contribute to unchanged rotation. A rank is not an
individual final level; it is recoupled to actual initial/final levels.
All blends use $s(x)=u^2(3-2u)$ with $u=\min(1,\max(0,x))$.

| Energy | Rotational source kernel | Applicability/assumption |
|---|---|---|
| Up to 1 eV | Transition-specific quadrupole/polarization Born kernel | Long-range sub-eV approximation |
| 1–20 eV | Log-energy mixture with a positive moment-constrained completion | Modeled bridge, not a measured resonance DCS |
| 20–200 eV | Bhattacharyya integral and momentum-transfer constraints under IAA’s angular marginal | Theoretical source without exchange; incomplete DCS information |
| 200–1000 eV | Equal-momentum-transfer continuation blended toward spectator | Modeled high-energy connection |
| At least 1000 eV | Bounded thermally recoupled spectator spectrum | Sudden approximation; finite-gap reciprocity approximate |

## Low-energy Born angular kernel

For each allowed N→N+2, use its own gap $\Delta_N=B(4N+6)$ and momenta,

$$k_i=\sqrt{2E/E_h},\quad k_f=\sqrt{2\max(E-\Delta_N,0)/E_h},$$
$$q_N=\sqrt{k_i^2+k_f^2-2k_ik_f\mu}.$$

The reduced strength from thesis Eq. 11.21b is

$$B_N(E,\mu)=\frac65\frac{(N+1)(N+2)}{(2N+1)(2N+3)}
\left(\frac{Q}{3}+\frac{\pi\alpha_2q_N}{32}\right)^2a_0^2,
\qquad Q=-0.29,\quad\alpha_2=4.93.$$

This comes from the anisotropic potential $V_2(r)=-Q/r^3-\alpha_2/(2r^4)$.
The Born radial integral uses
$\int_0^\infty j_2(qr)\,dr/r=1/3$ and
$\int_0^\infty j_2(qr)\,dr/r^2=\pi q/16$.
Thus quadrupole and polarization amplitudes add before squaring; their
interference must not be replaced by a sum of two independent probabilities.
The angular prefactor is $(4/5)C_{N,N+2,2}$. Applying the long-range potential
over the full integral neglects short-range and exchange/resonance effects.

Q and α₂ are in atomic units. The Born DCS is $(k_f/k_i)B_N$; the shared
production primitive supplies the threshold factor with the common
relativistic momentum convention, a negligible difference in the sub-eV region.
This evaluates the actual momentum transfer separately for every transition.
It is not obtained by rescaling one ground-state angular curve. O₂ has no
permanent dipole; this expression includes quadrupole and anisotropic induced
polarization.

## Completing the intermediate-energy rotational DCS

The numerical source is `../reciprocal_sources/o2_bhattacharyya.json`,
Bhattacharyya and Goswami (1983), Tables II–III, potential B with 2a₀ cutoff.
Columns are 1→1, 1→3 and the rotationally inclusive total, with both integral
and momentum-transfer cross sections. These are theoretical calculations,
not a complete measured rotational DCS set.

At actual energy E set $\widehat E=\min(200,\max(20,E))$.
The paper’s cross sections interpolate log–log at $\widehat E$ and the
spectator prior uses that same energy. The IAA angular marginal and total use
**actual E**. Therefore in the 1–20 eV bridge the paper’s 20 eV constraints
are fixed while the angular completion changes with E.

Define the normalized rank prior

$$S_L(\widehat E,y)=\frac{(2L+1)j_L(z)^2}{\sum_{K=0,2,\ldots,48}(2K+1)j_K(z)^2},
\quad z=\frac{2.281P(\widehat E)}{\alpha m_ec^2}y,$$
$$S_L^0=\int F(E,\theta)S_L(\widehat E,\theta)d\Omega,\quad
M_L^0=\int(1-\mu)F(E,\theta)S_L(\widehat E,\theta)d\Omega.$$

Recoupling from N=1 gives

$$\sigma_{11}=a_0+\tfrac25a_2,\quad
\sigma_{13}=\tfrac35a_2+\tfrac49a_4,$$
$$\sigma_{\rm inc}-\sigma_{11}-\sigma_{13}=\tfrac59a_4+\sum_{L\ge6}a_L.$$

The same identities hold for momentum-transfer integrals mL.
Let $R_\sigma=\sigma_{\rm inc}^p-\sigma_{11}^p-\sigma_{13}^p$ and
$R_m=m_{\rm inc}^p-m_{11}^p-m_{13}^p$ for the paper’s values, and set
$f_0=f_2=0$, $f_4=5/9$, $f_{L\ge6}=1$. For L≥4,

$$a_L=S_L^0\frac{R_\sigma}{\sum_K f_K S_K^0},\qquad
m_L=M_L^0\frac{R_m}{\sum_K f_K M_K^0}.$$

Then

$$a_2=\frac{\sigma_{13}^p-(4/9)a_4}{3/5},\qquad
m_2=\frac{m_{13}^p-(4/9)m_4}{3/5}.$$

This allocates the unresolved higher-rank budget using the spectator prior.
It does not assert that the paper uniquely determined that budget’s angular
shape. IAA still supplies the final absolute total, so σ11 from the paper is
not separately imposed on the final thermal model.

Use positive relative-entropy completion with $r_2=1$, $r_L=S_L$ for retained
higher ranks, and a rank-zero prior of one:

$$Z_\theta=1+\sum_{L>0}r_Le^{\lambda_L+\eta_L\mu},\quad
q_L=\frac{r_Le^{\lambda_L+\eta_L\mu}}{Z_\theta},\quad q_0=Z_\theta^{-1}.$$

The multipliers enforce

$$\int Fq_Ld\Omega=\frac{a_L}{\sigma_{\rm IAA}},\qquad
\int \mu Fq_Ld\Omega=\frac{a_L-m_L}{\sigma_{\rm IAA}}.$$

Equivalently minimize

$$\Phi=\int F\log Z_\theta d\Omega-
\sum_L\left[\lambda_La_L+\eta_L(a_L-m_L)\right]/\sigma_{\rm IAA}.$$

The angular strengths are $H_L=Iq_L$. The solver checks the achieved moments;
positivity alone is not a convergence test. Ranks below $10^{-13}$ of the
inclusive integral are omitted from this completion. This is a documented
inference from incomplete source information, not an added measurement.

## Exact bridge and normalization rules

Below 20 eV, define $w_O=s(\log(\max(E,1))/\log20)$, with E in eV. For f>i,

$$A_{if}^{\rm raw}=w_O\sum_{L>0}C_{ifL}H_L(E,\theta)
 +(1-w_O)\mathbf1_{f=i+2} B_i(E,\theta).$$

Thus E≤1 eV gives the transition-specific Born law; E=20 eV gives the full
moment-constrained completion. H(E) is fitted under the actual-energy IAA
marginal; this is not simply a fixed 20 eV angular curve multiplied by a weight.
For 20–200 eV the raw rank strengths are H(E).

The reciprocal solve protects changing primitives through 200 eV and fills the
unchanged remainder, including diagonal contributions from nonzero ranks.
It rejects a negative physical remainder. Over 200–220 eV it releases this
protected-background condition into a common paired normalization. That is a
normalization transition separate from the angular-shape blend below.

For 200–1000 eV take the completed 200 eV fractions $Q_L^{200}$, with rank zero
set to the inclusive remainder. Set $r=yP(E)/P(200)$, $y_a=\min(r,1)$,
$v=s((r-1)/0.05)$ and $w=s(\log(E/200)/\log5)$. Then

$$Q_L^{\rm map}=(1-v)Q_L^{200}(y_a)+vS_L(E,y),\qquad
A_L=I[(1-w)Q_L^{\rm map}+wS_L(E,y)].$$

The stored anchor interpolates linearly in y. This is equal-momentum-transfer
continuation, not evaluation at the same 200 eV scattering angle.

## Final DCS and kinetic channel selection

Let Xif be the corrected positive primitive after recoupling and the inclusive
solve, including the explicit transition-specific Born term where active.
For an excitation gap Δ>0,

$$D_{if}(E,\theta)=\frac{P(E-\Delta)}{P(E)^2}X_{if}(E,\theta),\qquad
D_{fi}(E,\theta)=\frac{g_i}{g_f}\frac{X_{if}(E+\Delta,\theta)}{P(E)}.$$

The unchanged DCS is $D_{ii}=\sum_LC_{iiL}X_L/P(E)$. Forward and reverse
share the corrected primitive; independent incoming-row renormalization is
not applied. Excitation is zero at/below its actual gap. The reverse rate uses
its finite zero-energy limit.

Above the cold join the inclusive constraint is $\sum_{if}b_iD_{if}=I$.
After drawing the angle from the elastic DCS, select

$$\Pr(i,f\mid E,\theta)=b_iD_{if}(E,\theta)/I(E,\theta).$$

The sampled Δ remains discrete, preserving heating, cooling and energy
diffusion. Below 1 meV use the explicit cold inclusive sum; emission at
exactly zero relative momentum is isotropic. There is no extra hard angular
accessibility cutoff. Recoil is applied afterward with the documented nominal
threshold continuation.

At/above 1 keV the bounded spectator probability is proportional to
$b_i\sum_LC_{ifL}(2L+1)j_L(z)^2$ over all bound final levels, through odd N=167.
The runtime uses momentum-transfer-bin averages and a phase-averaged extreme
tail. This is a high-energy sudden approximation, omitting finite-gap
reverse-energy corrections. Its discrete outcomes preserve energy variance.

The elastic angle separately transitions from the IAA shape to screened
Rutherford over 8–10 keV and retains its energy dependence above that. The
inclusive total uses the matched Born continuation above 6 keV. The combined
supported range ends at 1 GeV; source endpoints at 20 eV do not authorize
constant rotational tails.
