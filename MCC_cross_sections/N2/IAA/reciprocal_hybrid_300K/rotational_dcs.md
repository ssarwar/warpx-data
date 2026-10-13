# N₂ rotational angular model

This is the construction of the **rotational outcome DCS**, not merely the
elastic angle distribution. The complete derivation and code mapping are in
[WarpX’s rotational-DCS specification](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering/rotational_dcs.rst).
[The development guide](https://github.com/ssarwar/WarpX/blob/codex/rigid-beam-immobile-ions-development-sync/Docs/source/theory/multiphysics/rotational_scattering/development.rst)
gives the reproducible workflow for a new task or checkout.

## Definitions and physical interpretation

Energy E is in eV, DCS in m²/sr. Let

$$P(E)=\sqrt{E(E+2m_ec^2)},\quad y=\sin(\theta/2),\quad \mu=\cos\theta,$$
$$I(E,\theta)=\sigma_{\rm IAA}(E)F(E,\theta),\qquad \int F\,d\Omega=1.$$

I is the **thermally inclusive vibrationally elastic** DCS. It includes
unchanged, excitation and de-excitation outcomes. It is not a separate
rotationally unchanged channel. The residual integral determines its magnitude;
the normalized elastic DCS determines its angular shape.

An elementary rank L is recoupled to initial/final levels i,f through

$$C_{ifL}=|\langle i0,L0\mid f0\rangle|^2,$$
$$\Delta_{if}=B[f(f+1)-i(i+1)],\qquad B=0.0002477204284695341\ {
m eV}.$$

L is not generally the final level. Nonzero ranks can contribute to i=f.
The bath populations are $b_i\propto g_i e^{-\epsilon_i/k_BT}$, with even:odd
nuclear-spin weights 6:3 and $g_i=(2i+1)$ times that weight.
All blends use $s(x)=u^2(3-2u)$, $u=\min(1,\max(0,x))$.

## Energy regimes

| Energy | Angular source construction | Evidence/qualification |
|---|---|---|
| Up to 1 eV | Isotropic elementary rotational strengths | Leading low-energy approximation; real elmolcs/Itikawa and Kutz–Meyer integral strengths |
| 1–1.25 eV | Cubic blend to the resonance construction | Chosen interpolation |
| 1.25–4 eV | Kutz–Meyer strengths with Jung/Read angular multipliers | Two measured-energy anchors; background and missing angles modeled |
| 4–10 eV | Log-energy blend toward Gote fractions | Chosen interpolation into the 10 eV data |
| 10–200 eV | Completed Gote fractions under the IAA angular marginal | Measured angles 10–160°; spectator wings |
| 200–1000 eV | Equal-momentum-transfer continuation, blending to spectator | Modeled continuation |
| At least 1000 eV | Bounded thermally recoupled spectator spectrum | High-energy sudden approximation |

These are implemented intervals, not claims that every point has independent
experimental support. Kutz–Meyer’s elementary strengths are used in the
low/resonant model and its join; their absolute integrals are not additionally
imposed throughout the Gote/high-energy ranges.

## Low-energy and resonance strengths

For L=2,4,6 use the reduced elementary strength

$$U_L(E)=\sigma_{0L}(E)\frac{P(E)}{P(E-\Delta_{0L})},\qquad U_0=\sigma_{00}.$$

Positive reduced knots interpolate log–log. The first reduced value is held
below the first positive datum; each actual recoupled channel still uses its
own canonical threshold. The low-energy angular prior has
$A_0^{\rm low}=I$, $A_L^{\rm low}=U_L/(4\pi)$ for L=2,4,6, and other ranks zero.
The rank-zero placeholder is replaced by the inclusive-background solve.

The normalized Read–Andrick resonance shapes are

$$h_0=\frac{5(3\mu^2-1)^2}{16\pi},\quad
h_2=\frac{5(9\mu^4-9\mu^2+4)}{56\pi},\quad
h_4=\frac{5(\mu^2+3)^2}{224\pi}.$$

Each integrates to one. The temporary-anion resonance is treated as predominantly
²Πg, with capture/emission dominated by ℓ=2, |Λ|=1 partial waves. Coupling two
d waves allows even tensor ranks 0, 2 and 4; projection onto unpolarized rotor
states gives these polynomials. The approximation assumes a lifetime short
compared with molecular rotation and separable electronic/vibrational motion.
Rank six retained below is a separate source contribution. These are resonance tensors,
not a universal rotational DCS. The model includes direct/background scattering
through the Jung-constrained fractions; it does not use the pure tensors as
the entire vibrationally elastic cross section.

At 2.22 and 2.47 eV, `../reciprocal_sources/n2_jung.json` supplies inferred
rank-0,2,4 fractions on 15°,30°,…,105°. Those fractions come from thermal
branch digitization at 500 K, with documented reading uncertainty. Interpolate
linearly in angle; outside the measured interval use the endpoint fraction
times $h_L(\theta)/h_L(\theta_{\rm edge})$, then normalize the fractions.

For ranks 2 and 4 apply the missing-angle factor

$$1-(1-a_L)m(\theta),\quad
m=s((15^\circ-\theta)/15^\circ)+s((\theta-105^\circ)/15^\circ),$$
$$(a_2,a_4)=(0.52823198,0.84013224).$$

Assign the removed fraction to rank zero. These are adopted fit parameters
from the integral-branch constraint, not Read theory constants. Denote the
corrected anchor fractions by $r_L^a$. Then

$$H_L^a(\theta)=I(E_a,\theta)r_L^a(\theta)/U_L(E_a),$$
$$t_R=\min(1,\max(0,(E-2.22)/0.25)),\quad
H_L=(1-t_R)H_L^{2.22}+t_RH_L^{2.47},$$
$$A_L^{\rm res}=U_L(E)H_L(E,\theta)\quad(L=0,2,4),\qquad
A_6^{\rm res}=U_6/(4\pi).$$

For E≤4 eV, blend

$$A_L=(1-w_N)A_L^{\rm low}+w_NA_L^{\rm res},\qquad w_N=s((E-1)/0.25).$$

The angular multipliers are held outside their two anchor energies while the
Kutz–Meyer energy strengths continue to vary. Extending that construction
across the stated resonance interval is a model assumption.

## Intermediate-energy angular fractions

The completed Gote fractions $g_L$ interpolate linearly in angle and linearly
in log energy. Reported `<1%` values use 0.5%; inconsistent columns are
interpolated from consistent neighbors. The original percentages and absolute
DCS row remain in `../reciprocal_sources/n2_gote.json`; the latter is not used
as an extra absolute normalization. The detailed source qualifications are in
the adjacent source README.

The spectator rank prior is

$$S_L(E,y)=\frac{(2L+1)j_L(z)^2}{\sum_{K=0,2,\ldots,48}(2K+1)j_K(z)^2},
\qquad z=\frac{2.068P(E)}{\alpha m_ec^2}y.$$

The two-centre amplitude contains $e^{i\mathbf q\cdot\mathbf R/2}+e^{-i\mathbf q\cdot\mathbf R/2}$.
Its Rayleigh expansion contains only even L, with elementary weights
$(2L+1)j_L(qR/2)^2$. In the sudden elastic limit $q=2k\sin(\theta/2)$, explaining
the argument $z=kR\sin(\theta/2)$ and its factor of one half relative to qR.
The common atomic amplitude cancels in conditional channel probabilities.

Within 10–160°, $G_L=g_L$. Below 10° use
$G_L=[1-s(\theta/10^\circ)]S_L+s(\theta/10^\circ)g_L(E,10^\circ)$.
Above 160° use
$G_L=[1-s((\theta-160^\circ)/20^\circ)]g_L(E,160^\circ)+s((\theta-160^\circ)/20^\circ)S_L$.
The final fractions are normalized over ranks. Below 10 eV the measured-angle
fractions stay at 10 eV, but the spectator wings use the actual E.

For 4–10 eV,

$$A_L=(1-w_G)A_L^{\rm res}+w_G I G_L,\quad
w_G=s\left(\frac{\log(E/4)}{\log(10/4)}\right).$$

For 10–200 eV, $A_L=IG_L$.

## High-energy transition and final channel probabilities

For 200–1000 eV set $r=yP(E)/P(200)$, $y_a=\min(r,1)$,
$v=s((r-1)/0.05)$, and $w=s(\log(E/200)/\log5)$. Then

$$Q_L^{\rm map}=(1-v)G_L(200,y_a)+vS_L(E,y),$$
$$A_L=I[(1-w)Q_L^{\rm map}+wS_L].$$

This maps equal momentum transfer, rather than keeping the same angle at
200 eV. The additional r=1…1.05 join handles the edge of the anchor coverage.

Below 1 keV, the reciprocal inclusive solve turns $P(E)A_L$ into corrected
positive primitives $X_L$. With $X_{if}=\sum_LC_{ifL}X_L$,

$$D_{if}(E,\theta)=\frac{P(E-\Delta)}{P(E)^2}X_{if}(E,\theta),$$
$$D_{fi}(E,\theta)=\frac{g_i}{g_f}\frac{X_{if}(E+\Delta,\theta)}{P(E)}.$$

The unchanged contribution is $D_{ii}=\sum_LC_{iiL}X_L/P(E)$.
Excitation is zero at/below its gap. The changing kernels are protected through
1 eV; over 1–1.25 eV the normalization releases into its common-factor solve.
The reverse channel always uses its corrected forward primitive. An arbitrary
independent normalization of incoming channels would break detailed balance.

After reconciliation, $\sum_{if}b_iD_{if}=I$ above 1 meV. Thus the simulation
draws the elastic angle and then selects a channel with

$$\Pr(i,f\mid E,\theta)=b_iD_{if}(E,\theta)/I(E,\theta).$$

Below 1 meV the explicit cold inclusive rate replaces I in this expression;
at zero relative momentum emission is isotropic and the superelastic rate is
finite. See the main README and WarpX’s continuation equations.

At/above 1 keV, all bound-rotor ranks are included and the outcome probability
is proportional to $b_i\sum_L C_{ifL}(2L+1)j_L(z)^2$, normalized over all
allowed initial/final pairs. Final levels stop at J=197. The runtime uses
momentum-transfer-bin averages of probabilities and a resolved/phase-averaged
tail, preserving discrete energy diffusion. Finite-gap reverse-energy
corrections are neglected in this high-energy approximation.

The elastic-angle transition is separate: IAA through 8 keV, smooth mixing to
screened Rutherford over 8–10 keV, then its energy-dependent analytic inverse.
The total rate has its own matched Born continuation above 6 keV. All queries
are bounded by the supported 1 GeV endpoint. These distinctions are part of
the model; none of these joins is an arbitrary constant endpoint hold.
